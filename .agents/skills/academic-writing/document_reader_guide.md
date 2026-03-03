# Document Reader Guide — Hướng dẫn đọc file tài liệu

> Hướng dẫn Teacher đọc đa dạng file nguồn: PDF (có hình ảnh), Word (.docx), text.
> Agent đọc file này khi cần xử lý tài liệu đầu vào.

---

## 1. Tổng quan phương pháp đọc

| Loại file | Phương pháp | Output |
|:----------|:------------|:-------|
| **PDF có hình/sơ đồ** | Render trang → PNG + Extract text | Ảnh PNG + file text |
| **PDF toàn text** | Extract text trực tiếp | File text |
| **Word (.docx)** | Extract text + hình bằng python-docx | File text + ảnh |
| **Text / Markdown** | Đọc trực tiếp | — |

> **Nguyên tắc chung:** Luôn đọc **toàn bộ** tài liệu. Đọc chọn lọc có thể bỏ sót ngữ cảnh quan trọng.

---

## 2. Đọc PDF có hình ảnh ⭐

Đây là phương pháp **quan trọng nhất** — hầu hết tài liệu AIO2025 đều là PDF có sơ đồ, hình minh họa.

### Phương pháp: Render pages → ảnh + Extract text

**Bước 1:** Extract text để có bản text tìm kiếm nhanh bằng grep:

```python
import pymupdf

doc = pymupdf.open("input.pdf")
text = ""
for page in doc:
    text += page.get_text()
with open("{topic}_extracted.txt", "w", encoding="utf-8") as f:
    f.write(text)
print(f"Extracted {len(text)} chars from {len(doc)} pages")
```

**Bước 2:** Render **tất cả** các trang ra PNG (dpi=200 cho chất lượng tốt):

```python
import pymupdf, os

doc = pymupdf.open("input.pdf")
os.makedirs("{topic}_pages", exist_ok=True)
for i, page in enumerate(doc):
    pix = page.get_pixmap(dpi=200)
    pix.save(f"{topic}_pages/page_{i+1}.png")
print(f"Rendered {len(doc)} pages to {topic}_pages/")
```

**Bước 3:** Agent gọi tool `view_file` để đọc **hết** từng trang ảnh PNG → nắm toàn bộ nội dung bao gồm sơ đồ, bảng, công thức.

**Bước 4:** Ghi chú visual notes cho hình quan trọng:

```markdown
## Visual Notes — {topic}

### Trang 1
- Hình 1: [mô tả sơ đồ/hình]
- Nội dung chính: [...]

### Trang 5
- Hình 2: Sơ đồ kiến trúc [X]
  - Input: [...]
  - Process: [...]
  - Output: [...]
```

### Phương pháp tối ưu: Chỉ render trang có hình

Nếu PDF quá dài, có thể chỉ render các trang chứa hình:

```python
import pymupdf, os

doc = pymupdf.open("input.pdf")

# A: Extract toàn bộ text
text = ""
for page in doc:
    text += page.get_text()
with open("{topic}_extracted.txt", "w", encoding="utf-8") as f:
    f.write(text)

# B: Render chỉ các trang có hình
os.makedirs("{topic}_pages", exist_ok=True)
rendered = 0
for i, page in enumerate(doc):
    blocks = page.get_text("dict")["blocks"]
    has_image = any(b["type"] == 1 for b in blocks)
    if has_image:
        pix = page.get_pixmap(dpi=200)
        pix.save(f"{topic}_pages/page_{i+1}.png")
        rendered += 1
print(f"Rendered {rendered}/{len(doc)} pages (only pages with images)")
```

> **Lưu ý:** Phương pháp này nhanh hơn nhưng có thể bỏ sót các sơ đồ được vẽ bằng vector (ví dụ TikZ trong LaTeX). Nếu nghi ngờ, render **tất cả** trang.

---

## 3. Đọc PDF toàn text

Dùng khi PDF chủ yếu là văn bản, ít/không có hình ảnh quan trọng.

```python
import pymupdf

doc = pymupdf.open("input.pdf")
text = ""
for page in doc:
    text += page.get_text()
with open("{topic}_extracted.txt", "w", encoding="utf-8") as f:
    f.write(text)
print(f"Extracted {len(text)} chars from {len(doc)} pages")
```

**Đầu ra:** `{topic}_extracted.txt`

---

## 4. Đọc Word (.docx)

### Extract text + hình ảnh

```python
import os

# Cài đặt nếu chưa có
try:
    from docx import Document
except ImportError:
    import subprocess
    subprocess.check_call(['pip3', 'install', 'python-docx', '-q'])
    from docx import Document

from docx.opc.constants import RELATIONSHIP_TYPE as RT

doc = Document("input.docx")

# --- Extract text ---
text = ""
for para in doc.paragraphs:
    text += para.text + "\n"

# Extract text từ tables
for table in doc.tables:
    for row in table.rows:
        row_text = " | ".join(cell.text.strip() for cell in row.cells)
        text += row_text + "\n"
    text += "\n"

with open("{topic}_extracted.txt", "w", encoding="utf-8") as f:
    f.write(text)

# --- Extract hình ảnh ---
os.makedirs("{topic}_images", exist_ok=True)
img_count = 0
for rel in doc.part.rels.values():
    if "image" in rel.reltype:
        img_data = rel.target_part.blob
        ext = rel.target_part.content_type.split("/")[-1]
        if ext == "jpeg":
            ext = "jpg"
        img_path = f"{topic}_images/image_{img_count + 1}.{ext}"
        with open(img_path, "wb") as f:
            f.write(img_data)
        img_count += 1

print(f"Extracted {len(text)} chars text + {img_count} images")
```

**Đầu ra:**
- `{topic}_extracted.txt` — text content
- `{topic}_images/` — extracted images

### Extract structured content (advanced)

Nếu cần giữ cấu trúc (heading, bold, italic):

```python
from docx import Document

doc = Document("input.docx")

structured = ""
for para in doc.paragraphs:
    # Detect headings
    if para.style.name.startswith("Heading"):
        level = para.style.name.replace("Heading ", "")
        structured += f"\n{'#' * int(level)} {para.text}\n\n"
    else:
        # Detect bold/italic runs
        line = ""
        for run in para.runs:
            text = run.text
            if run.bold:
                text = f"**{text}**"
            if run.italic:
                text = f"*{text}*"
            line += text
        structured += line + "\n"

with open("{topic}_structured.md", "w", encoding="utf-8") as f:
    f.write(structured)
```

---

## 5. Chiến lược đọc

### Đọc toàn bộ, không đọc chọn lọc

| ❌ Đọc chọn lọc | ✅ Đọc toàn bộ |
|:-----------------|:---------------|
| Chỉ đọc phần liên quan → bỏ sót bối cảnh | Đọc hết → nắm toàn cảnh |
| Grep keyword → miss context | Render + đọc từng trang → thấy hình |
| Assume biết nội dung → hiểu sai | Đọc kỹ → hiểu đúng |

### Quy trình đọc khuyến nghị

```
1. Extract text (để grep tìm kiếm nhanh)
       │
       ▼
2. Render pages ra PNG (nếu có hình)
       │
       ▼
3. Đọc từng trang PNG bằng view_file
       │    (nắm hình, sơ đồ, bảng)
       ▼
4. Ghi chú visual notes
       │    (hình quan trọng, kiến trúc)
       ▼
5. Dùng text extract để grep chi tiết
       │    (công thức, tên biến, code)
       ▼
Output: Kiến thức đầy đủ
```

### Ghi chú khi đọc

Format ghi chú cho mỗi tài liệu:

```markdown
# Ghi chú đọc — {tên tài liệu}

## Tổng quan
- Loại: [Tutorial / Paper / Documentation]
- Số trang: [X]
- Chủ đề chính: [...]

## Kiến thức trọng tâm
1. [Concept 1] — trang [X]
2. [Concept 2] — trang [Y]

## Hình/Sơ đồ quan trọng
- Hình [X] (trang [N]): [mô tả] — dùng để [mục đích]
- Sơ đồ [Y] (trang [M]): [mô tả kiến trúc]

## Code mẫu
- Trang [X]: [mô tả code]

## Ghi chú thêm
- [...]
```

---

## 6. Xử lý lỗi thường gặp

| Lỗi | Nguyên nhân | Cách xử lý |
|:----|:------------|:-----------|
| `pymupdf` chưa cài | Chưa install | `pip3 install pymupdf` |
| `python-docx` chưa cài | Chưa install | `pip3 install python-docx` |
| PDF encrypted | File bị khóa | Yêu cầu user cung cấp password hoặc file không mã hóa |
| Text extract bị lỗi font | PDF dùng font đặc biệt | Render → PNG → đọc bằng view_file |
| Word file bị corrupt | File hỏng | Thử mở bằng LibreOffice → save lại |
| Hình trong Word bị embedded | Embedded OLE | Dùng `doc.part.rels` để extract |

## 7. Bảng so sánh phương pháp

| Phương pháp | Text | Hình | Cấu trúc | Tốc độ | Khi nào dùng |
|:------------|:----:|:----:|:---------:|:------:|:-------------|
| PDF Extract text | ✅ | ❌ | ❌ | Nhanh | PDF toàn chữ |
| PDF Render PNG | ✅ | ✅ | ❌ | Trung bình | PDF có hình |
| PDF Kết hợp | ✅ | ✅ | ❌ | Trung bình | Tài liệu dài |
| Word Extract | ✅ | ✅ | ✅ | Nhanh | File .docx |
| Word Structured | ✅ | ✅ | ✅ | Nhanh | Cần giữ format |
