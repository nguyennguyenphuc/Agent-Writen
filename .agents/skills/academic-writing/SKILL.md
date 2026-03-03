---
description: Quy trình viết bài học thuật (tutorial, tài liệu giảng dạy) theo chuẩn AIO2025 — bao gồm research, đọc tài liệu, viết bài và xuất LaTeX
---

# Academic Writing System — Viết bài học thuật AIO2025

Skill này mô tả quy trình sử dụng **Teacher** và **Student** agent để nghiên cứu, viết, review và chỉnh sửa bài viết học thuật (tutorial, tài liệu giảng dạy) theo chuẩn phong cách AIO2025.

- **Teacher** — viết bài, research, xuất LaTeX
- **Student** — đọc và review bài viết từ góc độ sinh viên (đầy đủ? dễ hiểu? code chạy?)

## Chế độ hoạt động

Skill có **4 chế độ chính**, chọn dựa trên input:

| Chế độ | Khi nào dùng | Input | Output |
|:-------|:-------------|:------|:-------|
| **RESEARCH** | Cần nghiên cứu chủ đề trước khi viết | Tên chủ đề / từ khóa | `{topic}_research_notes.md` |
| **CREATE** | Viết bài mới từ đầu | Tài liệu gốc (PDF/Word/text) hoặc research notes | Bài viết hoàn chỉnh (MD/LaTeX) |
| **REWRITE** | Chỉnh sửa / cải thiện bài có sẵn | Bài viết cần sửa + feedback | Bài viết đã cải tiến |
| **REVIEW** | Review chất lượng bài viết | Bài viết cần review | Báo cáo review + đề xuất |

---

## Quy trình tổng quan

```
  Đọc experience logs (Bước 0)
       │
       ▼
  RESEARCH (tùy chọn)
  ┌─────────────────────┐
  │ Tìm nguồn           │
  │ Đọc tài liệu gốc   │
  │ Tổng hợp kiến thức  │
  └─────────────────────┘
       │
       ▼
  Đọc tài liệu gốc (PDF/Word/text)
       │
       ▼
  Phân tích cấu trúc + lên outline
       │
       ▼
  Teacher viết bài từng section
       │
       ▼
  Teacher self-review
       │
       ▼
  ┌──────────────┐     ┌──────────────┐
  │   TEACHER    │◄───►│   STUDENT    │
  │  (Sửa bài   │     │  (Review:    │
  │   theo FB)   │     │   dễ hiểu?   │
  └──────────────┘     │   đầy đủ?)   │
       │               └──────────────┘
       ▼
  Xuất output (Markdown / LaTeX)
       │
       ▼
  Cập nhật experience logs (Bước cuối)
```

## Bước 0: Đọc kinh nghiệm tích lũy

**Luôn thực hiện ĐẦU TIÊN trước mọi bước khác.**

Đọc các file để nắm kinh nghiệm và phong cách từ những lần viết trước:

1. **`writing_teacher_experience.md`** — Lỗi đã mắc, mẫu viết hiệu quả, mẹo viết bài
2. **`reading_student_experience.md`** — Vấn đề Student đã phát hiện, checklist mở rộng
3. **`writing_style_guide.md`** — Phong cách viết AIO2025: cấu trúc, hành văn, patterns
4. **`writing_teacher_guide.md`** — Vai trò, tính cách, nhiệm vụ của Teacher viết bài

Mục đích: **không lặp lại lỗi cũ**, áp dụng pattern và phong cách đã chứng minh hiệu quả.

## Bước 1: Research chủ đề (tùy chọn)

Nếu cần nghiên cứu trước khi viết, đọc **`research_guide.md`** và thực hiện:

1. **Xác định phạm vi** — chủ đề cụ thể, mức độ chi tiết, đối tượng đọc
2. **Tìm nguồn** — web search, paper, documentation, GitHub repos
3. **Đọc & tổng hợp** — ghi chú key concepts, so sánh phương pháp
4. **Lên outline sơ bộ** — xác định flow bài viết

**Đầu ra:** `{topic}_research_notes.md`

## Bước 2: Đọc tài liệu gốc

Đọc tài liệu nguồn được cung cấp. Đọc **`document_reader_guide.md`** để biết cách xử lý từng loại file:

| Loại file | Phương pháp |
|:----------|:------------|
| **PDF có hình** | Render trang → PNG (dpi=200) → đọc qua `view_file` + extract text |
| **PDF toàn text** | Extract text bằng `pymupdf` |
| **Word (.docx)** | Extract text + hình bằng `python-docx` |
| **Text/Markdown** | Đọc trực tiếp |

> **Quan trọng:** Luôn đọc **toàn bộ** tài liệu. Đọc chọn lọc có thể bỏ sót ngữ cảnh quan trọng.

**Đầu ra:** `{topic}_extracted.txt` + `{topic}_pages/` (nếu có hình)

## Bước 3: Phân tích cấu trúc & Lên outline

Từ tài liệu đã đọc + research notes:

1. **Xác định kiến thức trọng tâm** — liệt kê concepts, công thức, kiến trúc
2. **Xác định cấu trúc bài viết** — theo mẫu AIO2025:
   ```
   I.   Giới thiệu (bối cảnh, tại sao quan trọng, bài toán cụ thể)
   II.  Kiến thức nền (lý thuyết, kiến trúc, công thức)
   III. Cài đặt / Thực hành (step-by-step với code)
   IV.  Câu hỏi trắc nghiệm (tùy chọn)
   V.   Tài liệu tham khảo
   Phụ lục (datasets, hints, solutions)
   ```
3. **Lên outline chi tiết** — mỗi section có: mục tiêu, nội dung chính, hình/bảng cần có

**Đầu ra:** `{topic}_outline.md`

## Bước 4: Viết bài

Viết từng section theo outline, tuân thủ **`writing_style_guide.md`**:

### Nguyên tắc viết

- **Giọng văn**: Thân mật, dùng "chúng ta", tạo cảm giác cùng khám phá
- **Thuật ngữ**: Bold + giải nghĩa tiếng Việt lần đầu xuất hiện
- **Code**: Giải thích trước → Code block → Giải thích sau (bullet list)
- **Hình**: Hình trước → Caption → Giải thích tham chiếu
- **Dẫn dắt**: Từ đơn giản → phức tạp, mỗi bước xây trên bước trước
- **Hộp thông tin**: Khái niệm box, Tip box, Lưu ý box — đúng chỗ, đúng lúc

### Checklist mỗi section

- [ ] Có bối cảnh/giới thiệu mở đầu?
- [ ] Thuật ngữ mới được bold + giải nghĩa?
- [ ] Có hình minh họa / sơ đồ?
- [ ] Code có giải thích trước + sau?
- [ ] Câu nối chuyển tiếp sang section tiếp?
- [ ] **Không** mở đầu bằng định nghĩa khô khan?

## Bước 5: Self-review (Teacher)

Teacher tự review bài viết theo tiêu chí:

| Tiêu chí | Kiểm tra |
|:---------|:---------|
| Tính chính xác | Nội dung đúng với tài liệu gốc? Code chạy được? |
| Tính đầy đủ | Bao phủ đủ kiến thức trọng tâm? |
| Tính rõ ràng | Sinh viên đọc hiểu được? Có chỗ mơ hồ? |
| Phong cách | Đúng giọng AIO2025? Có đủ hình/bảng/code? |
| Layout | Cân đối các section? Không quá dài/ngắn? |

## Bước 5.5: Student review (vòng lặp debate)

Sau khi Teacher self-review, **Student** đọc bài viết và review từ góc độ sinh viên.

Đọc **`reading_student_guide.md`** và **`reading_student_experience.md`** trước khi review.

### Student kiểm tra 7 tiêu chí:

| # | Tiêu chí | Câu hỏi |
|:-:|:---------|:--------|
| 1 | **Tính dễ hiểu** | Đọc 1 lần có hiểu không? |
| 2 | **Tính đầy đủ** | Có đủ kiến thức? Thiếu gì? |
| 3 | **Tính chính xác** | Nội dung đúng? Code chạy? |
| 4 | **Hình minh họa** | Đủ hình? Hình rõ ràng? |
| 5 | **Code blocks** | Có giải thích? Thiếu import? |
| 6 | **Giọng văn** | Thân thiện? Đúng chuẩn AIO2025? |
| 7 | **Flow dẫn dắt** | Đơn giản → phức tạp? Mượt? |

### Quy trình:

1. Student review bài → xuất `review_{topic}.md` (điểm /35 + danh sách issues)
2. Teacher đọc review → sửa bài theo feedback
3. Nếu còn issue CRITICAL/CLARITY → Student review lại (tối đa 2-3 vòng)
4. Khi đạt yêu cầu → chuyển sang Bước 6 (Xuất output)

## Bước 6: Xuất output

Chọn **format** phù hợp:

> **Output luôn lưu vào thư mục project/workspace đang mở**, không lưu vào thư mục skill.

| Format | File output | Khi nào dùng |
|:-------|:-----------|:-------------|
| **Markdown** | `{topic}_tutorial.md` | Draft nhanh, review nội bộ |
| **LaTeX** | `{topic}_tutorial.tex` | Xuất bản chính thức, in ấn |

Khi xuất LaTeX, đọc **`latex_writing_guide.md`** để biết cách format cho từng loại nội dung:

| Nội dung | LaTeX pattern |
|:---------|:-------------|
| Section/Subsection | `\section{}`, `\subsection{}`, `\subsubsection{}` |
| Hình ảnh | `figurecardcaptionaivn` / `figurecaptionmodern` / `\caption{}` |
| Bảng | `tcolorbox` + `tabularx` / `tabular` |
| Code Python | `aivncodebox` + `lstlisting` |
| Hộp khái niệm | `\begin{conceptbox}` |
| Hộp tip | `\begin{tipbox}` |
| Hộp lưu ý | `\begin{notebox}` |
| Công thức | `$...$` inline, `\[...\]` display |
| Tham khảo | BibTeX với `references.bib` |

> **Quan trọng:** Escape ký tự đặc biệt trong LaTeX: `_` → `\_`, `%` → `\%`, `&` → `\&`, `#` → `\#`

## Bước 7: Cập nhật kinh nghiệm

**Luôn thực hiện SAU KHI hoàn thành bài viết.**

### Cập nhật `writing_teacher_experience.md`:

- **Lỗi đã mắc:** Lỗi gì? Bối cảnh? Cách sửa? Bài học?
- **Mẫu viết hiệu quả:** Cấu trúc nào hoạt động tốt? Pattern nào được đánh giá cao?
- **Mẹo viết bài:** Kỹ thuật nào giúp bài viết rõ ràng, dễ hiểu hơn?

### Cập nhật `reading_student_experience.md`:

- **Vấn đề đã phát hiện:** Issue nào Student tìm được? Loại nào (CRITICAL/CLARITY/COMPLETENESS)?
- **Checklist mở rộng:** Có điểm kiểm tra mới nào cần thêm?
- **Lỗi review thường gặp:** Student bỏ sót vấn đề gì?

> **Nguyên tắc:** Viết ngắn gọn, cụ thể, có thể áp dụng ngay cho lần viết tiếp theo. Không viết chung chung.

---

## Quy trình REVIEW (bài có sẵn)

### 3 cách review:

| Cách | Mô tả | Khi nào dùng |
|:-----|:------|:-------------|
| **Review đầy đủ** | Student review → Teacher sửa → lặp | Cần review kỹ, có thời gian |
| **Teacher-only** | Chỉ Teacher review | Chỉ cần góc nhìn người viết |
| **Student-only** | Chỉ Student review (không sửa) | Chỉ cần góc nhìn người đọc |

### Review đầy đủ:

1. **Bước 0:** Đọc experience logs (cả Teacher + Student)
2. **Student đọc bài** — review theo 7 tiêu chí, xuất báo cáo
3. **Teacher đọc báo cáo** — phân tích feedback, sửa bài
4. **Student review lại** (nếu còn issue CRITICAL/CLARITY)
5. **Xuất báo cáo cuối** `review_{topic}.md`
6. **Cập nhật** experience logs (cả Teacher + Student)

### Teacher-only:

1. **Bước 0:** Đọc experience logs
2. **Đọc bài viết** có sẵn
3. **Đánh giá theo 5 tiêu chí** (chính xác, đầy đủ, rõ ràng, phong cách, layout)
4. **Kiểm tra chi tiết:** code, hình/bảng, thuật ngữ, giọng văn
5. **Đề xuất cải tiến** — sửa gì? thêm gì? bỏ gì?
6. **Xuất báo cáo** `review_teacher_{topic}.md`
7. **Cập nhật** experience logs

### Student-only:

1. **Bước 0:** Đọc Student experience logs
2. **Student đọc bài** — review theo 7 tiêu chí
3. **Xuất báo cáo** `review_student_{topic}.md` (điểm /35 + issues)
4. **Cập nhật** Student experience logs

## Quy trình REWRITE (chỉnh sửa bài có sẵn)

### Các bước:

1. **Bước 0:** Đọc experience logs
2. **Đọc bài viết gốc + feedback** (nếu có)
3. **Xác định phần cần sửa** — liệt kê cụ thể
4. **Sửa từng phần** — giữ nguyên phần tốt, chỉ sửa phần cần thiết
5. **Self-review** phần đã sửa
6. **Xuất bài viết đã cải tiến**
7. **Cập nhật** experience logs
