# Conversation History — Academic Writing Skill

> **Ngày:** 2026-03-03
> **Mục tiêu:** Tạo Teacher + Student agent chuyên viết bài học thuật theo chuẩn AIO2025

---

## 1. Yêu cầu ban đầu

User muốn tạo **teacher mới chuyên viết học thuật** với:
- Văn phong tham khảo từ `Tai lieu mau.pdf` (tutorial smolagents — AIO2025)
- Hướng dẫn LaTeX từ `Guide_Latex.pdf` + `Guide_Latex-2.pdf`
- Kế thừa cách viết từ teacher hiện tại: `teacher_guide.md`, `teacher_experience.md`

## 2. Quá trình thực hiện

### Bước 1: Phân tích tài liệu

Đã đọc và phân tích 8 file tham khảo:

| File | Nội dung rút ra |
|:-----|:---------------|
| `Tai lieu mau.pdf` (28 trang) | Cấu trúc bài viết AIO2025, giọng "chúng ta", pattern Giải thích→Code→Giải thích, Hình→Caption→Giải thích |
| `Guide_Latex.pdf` (6 trang) | Template LaTeX cơ bản: section, info boxes (khái niệm, tip, lưu ý), bảng ký hiệu, code block |
| `Guide_Latex-2.pdf` (12 trang) | 3 kiểu figure caption, 4 kiểu table, `aivncodebox`, 6 cặp màu accent/row |
| `teacher_guide.md` | Tính cách: thân thiện, kiên nhẫn, thực tế. Giọng giảng bài |
| `teacher_experience.md` | Format nhật ký kinh nghiệm: lỗi đã mắc, mẫu hiệu quả, mẹo |
| `SKILL.md` (teacher-student-debate) | Quy trình debate: CREATE/REVIEW, vòng lặp Teacher↔Student |
| `exam_style_analysis.md` | Hành văn AIO2025, patterns viết, checklist chất lượng |
| `latex_template.md` | LaTeX patterns cho đề thi: câu hỏi, hình, code, solution box |

### Bước 2: Tạo plan → User approve

Plan gồm 7 file + 1 workflow, sau đó user yêu cầu thêm:
- **Research theo chủ đề** (tìm web, đọc paper)
- **Đọc PDF có hình ảnh** (render PNG) + **Word (.docx)**

### Bước 3: Tạo 7 file skill gốc (Teacher)

| # | File | Size | Chức năng |
|:-:|:-----|:----:|:---------|
| 1 | `SKILL.md` | 11.9KB | Quy trình chính: RESEARCH/CREATE/REWRITE/REVIEW |
| 2 | `writing_teacher_guide.md` | 7.0KB | Vai trò, tính cách, triết lý viết bài |
| 3 | `writing_style_guide.md` | 9.3KB | Phong cách AIO2025: patterns, hành văn, checklist |
| 4 | `research_guide.md` | 5.1KB | Research chủ đề: tìm nguồn, tổng hợp, outline |
| 5 | `document_reader_guide.md` | 8.4KB | Đọc PDF có hình (render PNG), Word (.docx) |
| 6 | `latex_writing_guide.md` | 10.3KB | LaTeX: 3 kiểu hình, 4 kiểu bảng, code, hộp |
| 7 | `writing_teacher_experience.md` | 0.7KB | Nhật ký Teacher (khởi tạo trống) |

### Bước 4: Thêm Student reviewer

User yêu cầu thêm Student để review bài viết sau khi viết xong.

| # | File | Size | Chức năng |
|:-:|:-----|:----:|:---------|
| 8 | `reading_student_guide.md` | 6.7KB | 7 tiêu chí review, 5 mức severity, report format (/35) |
| 9 | `reading_student_experience.md` | 0.8KB | Nhật ký Student (khởi tạo trống) |

**Cập nhật SKILL.md:**
- Thêm Bước 5.5: Student review (vòng lặp debate)
- 3 chế độ review: đầy đủ / teacher-only / student-only

### Workflow

File: `.agents/workflows/academic-writing.md` — kích hoạt bằng `/academic-writing`

## 3. Cấu trúc cuối cùng

```
.agents/
├── skills/academic-writing/         ← SKILL MỚI (9 files)
│   ├── SKILL.md
│   ├── writing_teacher_guide.md
│   ├── writing_style_guide.md
│   ├── research_guide.md
│   ├── document_reader_guide.md
│   ├── latex_writing_guide.md
│   ├── writing_teacher_experience.md
│   ├── reading_student_guide.md
│   └── reading_student_experience.md
│
├── skills/teacher-student-debate/   ← SKILL CŨ (không thay đổi)
│   ├── SKILL.md
│   ├── teacher_guide.md
│   ├── teacher_experience.md
│   ├── student_guide.md
│   ├── student_experience.md
│   ├── exam_style_analysis.md
│   └── latex_template.md
│
└── workflows/
    ├── academic-writing.md          ← WORKFLOW MỚI
    └── teacher-student-debate.md
```

## 4. Quy trình hoạt động

```
Teacher đọc experience → Research (tùy chọn) → Đọc tài liệu (PDF/Word)
    → Lên outline → Viết từng section → Teacher self-review
    → Student review (đầy đủ? dễ hiểu? code chạy?)
    → Teacher sửa theo feedback → Lặp 2-3 vòng
    → Xuất output (Markdown/LaTeX)
    → Cập nhật experience logs (Teacher + Student)
```

## 5. Để resume

Khi tiếp tục trên máy khác:
1. Gọi `/academic-writing` để kích hoạt skill
2. Skill sẽ tự đọc tất cả guide files + experience logs
3. Có thể chỉnh sửa bất kỳ file guide nào để tinh chỉnh hành vi
4. Experience logs sẽ tự cập nhật sau mỗi lần viết bài

## 6. Tài liệu gốc tham khảo

Các file gốc đã dùng để xây dựng skill (giữ lại trong project):
- `Tai lieu mau.pdf` — Tutorial mẫu AIO2025
- `Guide_Latex.pdf` — Template LaTeX cơ bản
- `Guide_Latex-2.pdf` — Hướng dẫn LaTeX chi tiết
