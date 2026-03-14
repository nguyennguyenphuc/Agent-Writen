# Research Guide — Hướng dẫn nghiên cứu chủ đề

> Hướng dẫn Teacher research chủ đề trước khi viết bài.
> Dùng khi chưa có tài liệu gốc hoặc cần bổ sung kiến thức.

---

## 1. Khi nào cần Research?

| Tình huống | Research? | Lý do |
|:-----------|:---------:|:------|
| Có sẵn tài liệu gốc đầy đủ (PDF/Word) | ❌ | Chỉ cần đọc tài liệu gốc |
| Có tài liệu nhưng cần bổ sung kiến thức | ✅ Nhẹ | Tìm thêm nguồn liên quan |
| Chỉ có tên chủ đề, chưa có tài liệu | ✅ Đầy đủ | Cần tìm và tổng hợp từ đầu |
| Viết về công nghệ/thư viện mới | ✅ Đầy đủ | Documentation + examples |

## 2. Quy trình Research

```
1. Xác định phạm vi
       │
       ▼
2. Tìm nguồn
       │
       ▼
3. Đọc & Ghi chú
       │
       ▼
4. Tổng hợp kiến thức
       │
       ▼
5. Lên outline sơ bộ
       │
       ▼
Output: {topic}_research_notes.md
```

### Bước 1: Xác định phạm vi

Trước khi tìm kiếm, xác định rõ:

- **Chủ đề cụ thể:** VD: "smolagents framework" chứ không phải "AI agents nói chung"
- **Đối tượng đọc:** Sinh viên năm mấy? Đã biết gì? Cần biết gì?
- **Chiều sâu:** Tổng quan? Hướng dẫn thực hành? Phân tích chi tiết?
- **Phạm vi:** Chỉ lý thuyết? Có code? Có bài tập?

### Bước 2: Tìm nguồn

Ưu tiên tìm theo thứ tự:

| Thứ tự | Loại nguồn | Cách tìm | Ví dụ |
|:------:|:-----------|:---------|:------|
| 1 | **Documentation chính thức** | Web search tên thư viện/framework + "docs" | `smolagents documentation huggingface` |
| 2 | **GitHub repos** | Search GitHub, đọc README + examples | `github.com/huggingface/smolagents` |
| 3 | **Paper gốc** | Tìm trên arXiv, Google Scholar | `ReAct paper arXiv` |
| 4 | **Blog/Tutorial** | Web search chủ đề + "tutorial" hoặc "guide" | `smolagents tutorial` |
| 5 | **Course materials** | Tìm slides, notebooks từ các khóa học | `agents course huggingface` |

**Cách tìm kiếm hiệu quả:**

```
Dùng tool search_web hoặc read_url_content:

1. Tìm documentation:  "{topic} documentation official"
2. Tìm tutorial:       "{topic} tutorial guide"
3. Tìm paper:          "{topic} paper arXiv"
4. Tìm code:           "{topic} github example"
5. Tìm so sánh:        "{topic} vs {alternative} comparison"
```

### Bước 3: Đọc & Ghi chú

Khi đọc mỗi nguồn, ghi chú theo format:

```markdown
### Nguồn: [Tên nguồn]
- **URL:** [link]
- **Loại:** Documentation / Paper / Tutorial / Blog
- **Key concepts:**
  - Concept 1: [giải thích ngắn]
  - Concept 2: [giải thích ngắn]
- **Code mẫu:** [có/không] — [vị trí]
- **Hình minh họa:** [có/không] — [mô tả]
- **Liên quan đến:** [các nguồn/section khác]
```

### Bước 4: Tổng hợp kiến thức

Từ các ghi chú, tổng hợp thành:

1. **Danh sách concepts chính** — sắp xếp từ đơn giản → phức tạp
2. **Kiến trúc / Sơ đồ tổng quan** — vẽ hoặc mô tả bằng text
3. **So sánh phương pháp** (nếu có) — bảng so sánh ưu/nhược
4. **Code snippets quan trọng** — thu thập ví dụ cần dùng
5. **Câu hỏi mở** — điều gì chưa rõ? cần tìm thêm?

### Bước 5: Lên outline sơ bộ

Dựa trên kiến thức đã tổng hợp, lên outline bài viết:

```markdown
# {topic} — Outline sơ bộ

## I. Giới thiệu
- Bối cảnh: [X]
- Vấn đề: [Y]
- Hình tổng quan: [mô tả]

## II. Kiến thức nền
### II.1. [Concept 1]
- Nội dung: [...]
- Hình/Bảng: [...]
### II.2. [Concept 2]
- ...

## III. Thực hành
### III.1. Cài đặt
### III.2. [Bước 1]
### III.3. [Bước 2]
...

## IV. Câu hỏi (nếu cần)
## V. Tài liệu tham khảo
```

## 3. Output format

File `{topic}_research_notes.md` nên có cấu trúc:

```markdown
# Research Notes — {Topic}

## Phạm vi
- Chủ đề: [...]
- Đối tượng: [...]
- Chiều sâu: [...]

## Nguồn đã đọc
### 1. [Tên nguồn 1]
- URL: [...]
- Key concepts: [...]

### 2. [Tên nguồn 2]
- ...

## Kiến thức tổng hợp
### Concepts chính
1. [...]
2. [...]

### Kiến trúc / Sơ đồ
[...]

### So sánh (nếu có)
| A | B |
|...|...|

## Outline sơ bộ
[...]

## Câu hỏi mở
- [...]
```

## 4. Mẹo research

1. **Đọc README trước** — GitHub repo thường có tổng quan tốt nhất
2. **Tìm "Getting Started"** — thường chứa ví dụ đơn giản nhất
3. **Đọc example/ folder** — code mẫu thường rõ hơn docs
4. **So sánh nhiều nguồn** — mỗi nguồn giải thích khác nhau, tổng hợp cho đầy đủ
5. **Ghi chú ngay khi đọc** — không đợi đọc hết mới ghi
6. **Xác định phiên bản** — library/framework thay đổi nhanh, note version cụ thể
7. **Chạy thử code mẫu** — verify code hoạt động trước khi đưa vào bài viết
