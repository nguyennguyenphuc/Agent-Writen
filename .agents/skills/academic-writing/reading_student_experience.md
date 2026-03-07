# Reading Student — Nhật ký kinh nghiệm

> File này được agent tự động cập nhật sau mỗi lần review bài viết.
> **QUAN TRỌNG:** Đọc file này ĐẦU TIÊN trước khi review bài mới.

---

## Vấn đề đã phát hiện khi review

| # | Loại | Vấn đề | Cách Teacher đã sửa | Pattern cần chú ý |
|:-:|:----:|:-------|:--------------------|:-----------------|
| 1 | CRITICAL | 5/5 hình bài Basic Charts và 3/3 hình bài Preprocessing đều là placeholder | Cần chụp màn hình Tableau thực | Tableau tutorial thiếu hình = sinh viên không biết giao diện thực trông như thế nào |
| 2 | CRITICAL | Subsection III A→B→C→E→D→F (E trước D) | Đổi lại D↔E | Khi review: luôn kiểm tra thứ tự label |
| 3 | CLARITY | Typo: `táng` (Basic Charts, dòng 137), `trỪn` (Preprocessing, dòng 248) | Sửa thành `tháng`, `trên` | Đọc lướt phát hiện typo trước khi nộp |
| 4 | CLARITY | Cột "Trục Y" của Text Table trong bảng tổng kết Basic Charts ghi "Dimension" nhưng thực tế Text Table kéo Measure vào ô Text, không phải Rows | Cần sửa thành "Dimension/Measure (vào ô Text)" | Kiểm tra bảng tổng kết đối chiếu với nội dung thực tế bài |
| 5 | COMPLETENESS | Basic Charts thiếu giải thích "Compute Using → Cell" cho Stacked Bar 100% | Thêm giải thích Cell vs Table | Khi đề cập setting phức tạp, luôn giải thích tại sao chọn option đó |
| 6 | COMPLETENESS | `date_part = 'week'` không có trong bảng tham chiếu nhưng hàm `WEEK()` được dùng sau đó | Thêm vào bảng | Bảng tham chiếu phải nhất quán với nội dung sử dụng sau đó |

## Checklist mở rộng (học từ kinh nghiệm)

Ngoài checklist chuẩn trong `reading_student_guide.md`, thêm các điểm kiểm tra đã học:

1. **Kiểm tra thứ tự đánh số subsection** — A, B, C... hay 1, 2, 3... có đúng thứ tự không?
2. **Kiểm tra bảng tổng kết** đối chiếu với nội dung chi tiết trong bài — có mâu thuẫn không?
3. **Đếm số hình placeholder** — bài Tableau không được có hình placeholder khi nộp
4. **Kiểm tra mỗi setting/option phức tạp** có giải thích "tại sao chọn" không?
5. **Bảng tham chiếu** (như date_part) phải đồng nhất với cách dùng trong phần sau
6. **Section gộp 2+ chủ đề** — mỗi section chỉ 1 chủ đề rõ ràng

## Lỗi thường gặp khi review

1. **Đọc bỏ qua số thứ tự section** — nhìn vào nội dung mà quên kiểm tra số thứ tự A/B/C
2. **Không đối chiếu bảng tổng kết với nội dung bài** — dễ bỏ sót mâu thuẫn giữa 2 phần
3. **Bỏ qua hình placeholder** — tưởng là sẽ điền sau nên không flag, nhưng đây là CRITICAL nếu không có hình khi nộp
