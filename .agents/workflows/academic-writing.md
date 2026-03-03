---
description: Quy trình viết bài học thuật (tutorial, tài liệu giảng dạy) theo chuẩn AIO2025 — bao gồm research, đọc tài liệu, viết bài và xuất LaTeX
---

# Academic Writing Workflow

Workflow này kích hoạt skill **academic-writing** để viết bài học thuật theo chuẩn AIO2025.

## Cách sử dụng

Skill hỗ trợ 4 chế độ:

| Chế độ | Lệnh | Ví dụ |
|:-------|:------|:------|
| **RESEARCH** | `/academic-writing` + tên chủ đề | "Research về smolagents framework" |
| **CREATE** | `/academic-writing` + tài liệu gốc | "Viết tutorial từ file PDF này" |
| **REWRITE** | `/academic-writing` + bài viết + feedback | "Sửa lại phần II theo feedback" |
| **REVIEW** | `/academic-writing` + bài viết cần review | "Review bài tutorial này" |

## Quy trình

// turbo-all

1. Đọc SKILL.md để nắm quy trình tổng quan:
   ```
   Đọc file: .agents/skills/academic-writing/SKILL.md
   ```

2. Đọc kinh nghiệm tích lũy (Bước 0):
   ```
   Đọc file: .agents/skills/academic-writing/writing_teacher_experience.md
   Đọc file: .agents/skills/academic-writing/reading_student_experience.md
   Đọc file: .agents/skills/academic-writing/writing_style_guide.md
   Đọc file: .agents/skills/academic-writing/writing_teacher_guide.md
   ```

3. Nếu cần research, đọc hướng dẫn research:
   ```
   Đọc file: .agents/skills/academic-writing/research_guide.md
   ```

4. Nếu cần đọc file PDF/Word, đọc hướng dẫn đọc tài liệu:
   ```
   Đọc file: .agents/skills/academic-writing/document_reader_guide.md
   ```

5. Nếu cần xuất LaTeX, đọc hướng dẫn LaTeX:
   ```
   Đọc file: .agents/skills/academic-writing/latex_writing_guide.md
   ```

6. Thực hiện theo quy trình trong SKILL.md (Research → Đọc tài liệu → Outline → Viết → Teacher self-review → Student review → Sửa → Xuất)

7. Khi cần Student review, đọc hướng dẫn Student:
   ```
   Đọc file: .agents/skills/academic-writing/reading_student_guide.md
   ```

8. Cập nhật kinh nghiệm sau khi hoàn thành:
   ```
   Cập nhật file: .agents/skills/academic-writing/writing_teacher_experience.md
   Cập nhật file: .agents/skills/academic-writing/reading_student_experience.md
   ```
