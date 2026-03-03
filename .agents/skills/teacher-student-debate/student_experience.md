# Student — Nhật ký kinh nghiệm

> File này được agent tự động cập nhật sau mỗi lần review/giải đề.  
> **QUAN TRỌNG:** Đọc file này ĐẦU TIÊN trước khi phản biện đề mới.

---

## Vấn đề đã phát hiện khi review

| # | Loại | Vấn đề | Cách Teacher đã sửa | Pattern cần chú ý |
|:-:|:----:|:-------|:--------------------|:-----------------|
| 1 | 🟡 WARNING | Thứ tự phép toán (DW trước hay PW trước) không rõ | Thêm lưu ý rõ ràng trong đề | Luôn kiểm tra thứ tự các phép toán có được ghi rõ trong đề không |
| 2 | 🟡 WARNING | Vị trí (1,1) không ghi rõ 0-indexed hay 1-indexed | Chưa sửa (PDF version) | Luôn kiểm tra index convention khi đề hỏi giá trị tại vị trí cụ thể |
| 3 | 🟡 WARNING | Tiêu chí "vi phạm định nghĩa chuẩn" mơ hồ — chuẩn nào? | Đề xuất sửa thành "output không thể hiện đặc trưng riêng" | Khi đề dùng từ trừu tượng như "chuẩn", "chính thức" → ghi nhận là mơ hồ |
| 4 | 🟡 WARNING | Hình 1 object + 1 mask có thể là semantic hoặc instance seg | Chưa sửa — đề xuất thay hình hoặc thêm ghi chú | Khi hình minh họa có thể interpret nhiều cách → đề cần ghi rõ tiêu chí |
| 5 | 🔴 CRITICAL | GT=16 nhưng 1/16 tính thành 0.08 (đúng: 0.0625) → AP sai không có đáp án đúng | Cần sửa GT=12 hoặc tính lại | Khi thấy phân số chia ra lẻ (1/16), LUÔN kiểm tra bằng calculator — không tin rounding của đề |
| 6 | 🔴 CRITICAL | Câu hỏi phụ thuộc notebook link — không có data trong đề | Chưa sửa | Nếu đề nói "truy cập link" → flag CRITICAL vì không verify được |

## Checklist mở rộng (học từ kinh nghiệm)

Ngoài checklist chuẩn trong `student_guide.md`, thêm các điểm kiểm tra đã học:

1. **Thứ tự phép toán:** Khi đề có chuỗi phép toán (DW→PW, Encode→Decode...), kiểm tra thứ tự có được ghi rõ không
2. **Index convention:** Khi đề hỏi giá trị tại vị trí (i,j), kiểm tra 0-indexed hay 1-indexed có ghi rõ không
3. **Padding verification:** Khi có padding, vẽ rõ ma trận padded trước khi tinhà — dễ sai nếu tính nhẩm
4. **Tiêu chí đánh giá trừu tượng:** Khi đề dùng từ như "chuẩn", "chính thức", "hợp lệ" → ghi nhận là mơ hồ, yêu cầu cụ thể hóa
5. **Hình minh họa multi-interpretable:** Khi 1 hình/output có thể interpret nhiều cách (ví dụ: 1 mask = semantic hay instance?) → cần tiêu chí rõ ràng

## Lỗi thường gặp khi giải

*(sẽ bổ sung khi Student giải sai và nhận feedback)*
