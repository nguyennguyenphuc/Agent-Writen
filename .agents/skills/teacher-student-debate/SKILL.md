---
name: teacher-student-debate
description: Quy trình Teacher-Student debate để ra đề, review và tinh chỉnh đề thi cho bất kỳ chủ đề nào
---

# Teacher-Student Exam Debate System

Skill này mô tả quy trình sử dụng hai agent **Teacher** và **Student** để tạo, review và tinh chỉnh đề thi + đáp án cho bất kỳ chủ đề nào.

## Chế độ hoạt động

Skill có **2 chế độ chính**, chọn dựa trên input:

| Chế độ | Khi nào dùng | Input | Output |
|:-------|:-------------|:------|:-------|
| **CREATE** | Chưa có đề, cần ra đề mới | Tài liệu lý thuyết (PDF) | Đề + Solution mới |
| **REVIEW** | Đã có đề + solution, cần cải tiến | Đề + Solution có sẵn | Báo cáo review + Đề đã cải tiến |

### Chế độ CREATE → theo quy trình Bước 0-8 bên dưới

### Chế độ REVIEW → 3 cách chạy

| Cách | Mô tả | Khi nào dùng |
|:-----|:------|:-------------|
| **Review đầy đủ** | Student review → Teacher sửa → Student giải → Teacher chấm | Cần review kỹ, có thời gian |
| **Teacher-only** | Chỉ Teacher review đề + solution | Chỉ cần góc nhìn người ra đề |
| **Student-only** | Chỉ Student review đề (không xem solution) | Chỉ cần góc nhìn người giải |

---

## Quy trình REVIEW (đề có sẵn)

### Review đầy đủ (vòng lặp debate)

```
Đọc experience logs (Bước 0)
       │
       ▼
Đọc đề + solution có sẵn
       │
       ▼
┌──────────────┐     ┌──────────────┐
│   TEACHER    │◄───►│   STUDENT    │
│  (Review đề, │     │  (Phản biện, │
│   sửa đề)    │     │   giải thử)  │
└──────────────┘     └──────────────┘
       │
       ▼
  Báo cáo review + Đề cải tiến
       │
       ▼
  Cập nhật experience logs (Bước 8)
```

**Các bước:**
1. **Bước 0:** Đọc experience logs
2. **Đọc đề + solution** có sẵn
3. **Student phản biện đề** (không xem solution) — theo quy trình Bước 3
4. **Student giải bài** — theo quy trình Bước 5
5. **Teacher chấm** — so đáp án Student vs solution có sẵn
6. **Teacher đánh giá tổng thể** — review cả đề lẫn solution, đề xuất cải tiến
7. **Teacher sửa đề** (nếu cần) — theo quy trình Bước 4
8. **Xuất báo cáo** `review_{topic}.md` gồm: vấn đề tìm thấy + hướng cải tiến + đề đã sửa (nếu có)
9. **Bước 8:** Cập nhật experience logs

### Teacher-only review

**Các bước:**
1. **Bước 0:** Đọc experience logs
2. **Đọc đề + solution** có sẵn
3. **Teacher đánh giá** theo 5 tiêu chí (rõ ràng, ngắn gọn, chính xác, giải được, đúng lý thuyết)
4. **Teacher kiểm tra solution** — verify bằng Python, đáp án có đúng không?
5. **Teacher đề xuất cải tiến** — sửa gì? thêm gì? bỏ gì?
6. **Xuất báo cáo** `review_teacher_{topic}.md`
7. **Bước 8:** Cập nhật experience logs

### Student-only review

**Các bước:**
1. **Bước 0:** Đọc experience logs
2. **Đọc đề** (KHÔNG xem solution)
3. **Student phản biện** — theo checklist trong `student_guide.md` + `student_experience.md`
4. **Student giải bài** — step-by-step, ghi rõ chỗ mơ hồ
5. **(Tùy chọn)** So đáp án Student vs solution có sẵn → nếu sai, đề có vấn đề hay Student hiểu nhầm?
6. **Xuất báo cáo** `review_student_{topic}.md`
7. **Bước 8:** Cập nhật experience logs

---

## Quy trình CREATE (ra đề mới)

```
  Đọc experience logs (Bước 0)
       │
       ▼
Tài liệu gốc (PDF/text)
       │
       ▼
  Trích xuất kiến thức
       │
       ▼
┌──────────────┐     ┌──────────────┐
│   TEACHER    │◄───►│   STUDENT    │
│  (Ra đề,     │     │  (Giải bài,  │
│   chấm bài)  │     │   phản biện) │
└──────────────┘     └──────────────┘
       │
       ▼
  Đề + Solution hoàn chỉnh
       │
       ▼
  Cập nhật experience logs (Bước 8)
```

## Bước 0: Đọc kinh nghiệm tích lũy

**Luôn thực hiện ĐẦU TIÊN trước mọi bước khác.**

Đọc 3 file để nắm kinh nghiệm và phong cách từ những lần ra đề trước:

1. **`teacher_experience.md`** — Lỗi Teacher đã mắc, mẫu đề hiệu quả, mẹo ra đề
2. **`student_experience.md`** — Vấn đề Student đã phát hiện, checklist mở rộng, lỗi giải thường gặp
3. **`exam_style_analysis.md`** — Nguyên tắc phong cách ra đề và giải: cấu trúc bộ đề, format câu hỏi, cách viết solution

Mục đích: **không lặp lại lỗi cũ**, áp dụng pattern và phong cách đã chứng minh hiệu quả.

## Bước 1: Trích xuất kiến thức

Từ tài liệu gốc (PDF, slide, sách), trích xuất nội dung. Chọn phương pháp phù hợp:

### Phương pháp A: Extract text (PDF toàn chữ)

Dùng khi PDF chủ yếu là văn bản, ít/không có hình ảnh quan trọng.

```python
import pymupdf
doc = pymupdf.open("input.pdf")
text = ""
for page in doc:
    text += page.get_text()
with open("{topic}_theory_extracted.txt", "w", encoding="utf-8") as f:
    f.write(text)
```

**Đầu ra:** `{topic}_theory_extracted.txt`

### Phương pháp B: Render pages → ảnh (PDF có hình/sơ đồ) ⭐

Dùng khi PDF chứa **sơ đồ kiến trúc, hình minh họa, công thức dạng ảnh, bảng phức tạp** — những thứ mà text extraction sẽ bị mất.

```python
import pymupdf, os
doc = pymupdf.open("input.pdf")
os.makedirs("{topic}_pages", exist_ok=True)
for i, page in enumerate(doc):
    pix = page.get_pixmap(dpi=200)
    pix.save(f"{topic}_pages/page_{i+1}.png")
```

**Đầu ra:** Thư mục `{topic}_pages/` chứa ảnh PNG từng trang.

**Cách đọc sau khi render:**

1. Extract text trước (phương pháp A) để có bản text tìm kiếm nhanh bằng grep
2. Render **tất cả** các trang có chứa hình ra PNG
3. Agent gọi tool `view_file` (tool nội bộ của AI, không phải hàm Python) để **đọc hết** từng trang ảnh PNG → nắm toàn bộ nội dung bao gồm sơ đồ, bảng, công thức
4. Ghi chú thông tin quan trọng vào `{topic}_visual_notes.md`

> **Lý do đọc hết:** Agent cần nắm toàn bộ kiến thức trong tài liệu để ra đề chính xác và đầy đủ. Đọc chọn lọc có thể bỏ sót ngữ cảnh quan trọng.

### Phương pháp C: Kết hợp A + B (tối ưu nhất)

Dùng cho tài liệu dài, vừa có text vừa có hình:
1. **Extract text** (phương pháp A) để tìm kiếm nhanh, grep keyword
2. **Render ảnh** (phương pháp B) chỉ cho các trang chứa hình quan trọng

```python
import pymupdf, os
doc = pymupdf.open("input.pdf")

# A: Extract toàn bộ text
text = ""
for page in doc:
    text += page.get_text()
with open("{topic}_theory_extracted.txt", "w", encoding="utf-8") as f:
    f.write(text)

# B: Render các trang có hình (ví dụ: trang có ít text = nhiều hình)
os.makedirs("{topic}_pages", exist_ok=True)
for i, page in enumerate(doc):
    blocks = page.get_text("dict")["blocks"]
    has_image = any(b["type"] == 1 for b in blocks)
    if has_image:
        pix = page.get_pixmap(dpi=200)
        pix.save(f"{topic}_pages/page_{i+1}.png")
```

### Bảng so sánh

| Phương pháp | Text | Hình/Sơ đồ | Tốc độ | Khi nào dùng |
|:---|:---:|:---:|:---:|:---|
| A: Extract text | ✅ | ❌ | Nhanh | PDF toàn chữ |
| B: Render ảnh | ✅ | ✅ | Trung bình | PDF nhiều hình |
| C: Kết hợp | ✅ | ✅ | Trung bình | Tài liệu dài, hỗn hợp |

### Đầu ra bổ sung

Nếu có sẵn đề mẫu và đáp án, trích xuất thêm:
- `{topic}_quiz_extracted.txt`
- `{topic}_solution_extracted.txt`

## Bước 2: Teacher ra đề

Teacher đọc tài liệu lý thuyết, sau đó:

1. **Xác định các chủ đề chính** trong tài liệu
2. **Tạo câu hỏi** đa dạng (trắc nghiệm + tự luận) bao phủ các chủ đề
3. **Viết đáp án** + giải thích chi tiết cho mỗi câu
4. **Kiểm tra** đề có tính tay được không (nếu là bài tính toán)

### Nguyên tắc ra đề

- Đề phải **ngắn gọn**, không dài dòng định nghĩa — sinh viên đọc phải hiểu ngay
- Nếu cần đơn giản hóa mô hình: ghi rõ **"Để đơn giản cho việc tính toán, weight được cố định sao cho..."** rồi mô tả hiệu quả cụ thể
- Ưu tiên sơ đồ trực quan (ASCII diagram) thay vì mô tả bằng lời
- Nếu dùng quy ước khác chuẩn: phải nêu rõ + cho ví dụ cụ thể inline
- Dữ kiện chỉ cho đủ dùng, giải thích ngắn gọn tại sao chỉ cho bấy nhiêu

### Checklist đề tốt

- [ ] Sơ đồ kiến trúc/luồng dữ liệu rõ ràng?
- [ ] Quy tắc tính viết bằng lời tự nhiên (không trừu tượng)?
- [ ] Có ví dụ inline cho quy tắc phức tạp?
- [ ] Dữ kiện đầy đủ để giải?
- [ ] Câu hỏi rõ ràng, 1 đáp án duy nhất?
- [ ] Tính tay được trong thời gian hợp lý?

## Bước 3: Student phản biện đề

Student đọc đề (KHÔNG xem solution) và kiểm tra:

1. **Tính rõ ràng:** Có hiểu đề không? Có chỗ nào mơ hồ?
2. **Tính đúng đắn:** Đề có mâu thuẫn với lý thuyết không?
3. **Tính đầy đủ:** Dữ kiện có đủ để giải không?
4. **Thuật ngữ:** Có nhất quán không? Có mixing framework (TensorFlow/PyTorch) không?
5. **Giả định ngầm:** Có quy tắc nào không được nêu rõ?

### Phân loại vấn đề

| Mức | Mô tả | Ví dụ |
|:---:|:------|:------|
| 🔴 CRITICAL | Sai lý thuyết hoặc thiếu thông tin quan trọng | Skip connection dùng ADD thay vì CONCAT |
| 🟡 WARNING | Mơ hồ hoặc dễ hiểu nhầm | Không ghi rõ thứ tự phép tính |
| 🟢 MINOR | Vấn đề nhỏ, chấp nhận được | Thuật ngữ chưa thống nhất |

## Bước 4: Teacher sửa đề

Dựa trên phản biện của Student, Teacher sửa đề theo nguyên tắc:

- **Sửa ít nhất có thể** — chỉ sửa vấn đề thực sự
- **Giữ đáp án nếu có thể** — nếu thay đổi cách tính thì verify lại
- **Không thêm định nghĩa trừu tượng** — viết tự nhiên, cụ thể
- Sau mỗi lần sửa: **chạy Python verify** đáp án vẫn đúng

## Bước 5: Student giải bài

Student đọc đề đã sửa và **giải bài step-by-step**:

1. Đọc hiểu đề — ghi lại mọi thắc mắc
2. Xác định chiến lược giải
3. Tính toán từng bước, ghi rõ từng phép tính
4. Đưa ra đáp án cuối cùng

**Mục đích:** Test xem đề có đủ rõ để giải đúng không.

## Bước 6: Teacher chấm bài

Teacher so sánh đáp án Student vs đáp án đúng, đánh giá:

| Tiêu chí | Điểm |
|:---------|:----:|
| Độ rõ ràng | /5 |
| Độ ngắn gọn | /5 |
| Tính chính xác | /5 |
| Tính giải được | /5 |
| Đúng lý thuyết chuẩn | /5 |

Nếu Student giải SAI → quay lại Bước 4 sửa đề.
Nếu Student giải ĐÚNG → đề đạt yêu cầu.

## Bước 7: Xuất kết quả

Xuất đề + solution. Chọn **format** phù hợp:

> **Output luôn lưu vào thư mục project/workspace đang mở**, không lưu vào thư mục skill.

| Format | File output | Khi nào dùng |
|:-------|:-----------|:-------------|
| **Markdown** | `.md` | Draft nhanh, review nội bộ |
| **LaTeX** | `.tex` | Xuất bản chính thức, in ấn, nộp bài |

### Output Markdown

- `de_thi_{topic}.md` — Đề hoàn chỉnh
- `solution_{topic}.md` — Solution chi tiết

### Output LaTeX

- `de_thi_{topic}.tex` — Đề hoàn chỉnh
- `solution_{topic}.tex` — Solution chi tiết

Khi xuất LaTeX, **đọc `latex_template.md`** (cùng thư mục skill) để biết cách format cho từng loại nội dung:

| Nội dung | LaTeX pattern |
|:---------|:-------------|
| Câu hỏi + đáp án trắc nghiệm | `\noindent \textbf{Câu X.}` + `\begin{enumerate}[(a)]` |
| Hình ảnh / sơ đồ | `\includesvg` hoặc `\includegraphics` hoặc `tikzpicture` |
| Công thức toán | `$...$` inline, `\[...\]` display, `\begin{bmatrix}` ma trận |
| Code Python | `\begin{aivncodebox}` + `\begin{lstlisting}[language=Python]` |
| Solution box | `\begin{guidelinebox}` |
| Bảng | `\begin{tabular}` |
| Lưu ý | `\begin{notebox}` |
| Ngắt trang | `\newpage` |

> **Quan trọng:** Escape ký tự đặc biệt trong LaTeX: `_` → `\_`, `%` → `\%`, `&` → `\&`, `#` → `\#`

### Format đề Markdown (tham khảo)

```markdown
# Đề thi — [Tên chủ đề]

## Câu X.
[Mô tả ngắn gọn bài toán]

### Sơ đồ (nếu có)
[ASCII diagram]

### Quy tắc (nếu cần)
Để đơn giản..., weight/tham số được cố định sao cho...
- [Quy tắc 1 viết tự nhiên + ví dụ]
- [Quy tắc 2 viết tự nhiên + ví dụ]

### Dữ kiện
[Chỉ cho đủ dùng]

### Câu hỏi
[1 câu rõ ràng]
[Đáp án trắc nghiệm]
```

## Lặp lại (nếu cần)

Quy trình có thể lặp nhiều vòng:

```
Teacher ra đề → Student phản biện → Teacher sửa
→ Student giải → Teacher chấm → OK? → Xuất kết quả
                                  ↓ NOT OK
                              Quay lại sửa
```

Thường **2-3 vòng** là đủ để đề đạt chất lượng.

## Bước 8: Cập nhật kinh nghiệm

**Luôn thực hiện SAU KHI hoàn thành đề.** Đây là bước quan trọng để agent học hỏi qua mỗi lần.

Cập nhật 3 file:

### Cập nhật `teacher_experience.md`

Thêm vào cuối các bảng tương ứng:
- **Lỗi đã mắc:** Lỗi nào Student phát hiện? Teacher tự nhận ra? Cách đã sửa + bài học rút ra
- **Mẫu đề hiệu quả:** Cấu trúc đề nào hoạt động tốt? Student giải đúng ngay lần đầu?
- **Mẹo ra đề:** Kỹ thuật nào giúp đề rõ ràng hơn, dễ tính tay hơn?

### Cập nhật `student_experience.md`

Thêm vào cuối các bảng tương ứng:
- **Vấn đề đã phát hiện:** Issue nào Student tìm được khi review? Loại nào (CRITICAL/WARNING/MINOR)?
- **Checklist mở rộng:** Có điểm kiểm tra mới nào cần thêm vào checklist?
- **Lỗi giải thường gặp:** Student giải sai ở đâu? Tại sao?

### Cập nhật `exam_style_analysis.md`

Nếu phát hiện nguyên tắc phong cách mới qua quá trình ra đề:
- **Pattern mới:** Loại câu hỏi mới hiệu quả? Cách format đáp án mới?
- **Tinh chỉnh nguyên tắc cũ:** Nguyên tắc nào cần bổ sung, sửa đổi dựa trên thực tế?
- **Anti-pattern:** Phong cách nào đã chứng minh là kém hiệu quả?

> **Nguyên tắc:** Viết ngắn gọn, cụ thể, có thể áp dụng ngay cho lần ra đề tiếp theo. Không viết chung chung.
