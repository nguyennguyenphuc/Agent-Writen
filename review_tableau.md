# Review — Đề thi Tableau: Basic Charts & Preprocessing Functions

## Tổng quan

| Tiêu chí | Điểm |
|:---------|:----:|
| Độ rõ ràng | 5/5 |
| Độ ngắn gọn | 4/5 |
| Tính chính xác | 4/5 |
| Tính giải được | 5/5 |
| Đúng lý thuyết chuẩn | 4/5 |
| **Tổng** | **22/25** |

## Student giải bài: 4/4 đúng

| Câu | Student chọn | Đáp án | Kết quả |
|:---:|:------------:|:------:|:-------:|
| 1 | C | C | ✅ |
| 2 | B | B | ✅ |
| 3 | B | B | ✅ |
| 4 | B | B | ✅ |

## Điểm mạnh

1. **Bộ đề liên kết tốt** — Câu 1-2 tạo Calculated Fields → Câu 3 dùng field từ Câu 2 → Câu 4 tiếp tục phân tích. Tạo cảm giác làm lab thực tế.
2. **Distracters chất lượng** — Mỗi đáp án sai đều có logic: A sai 1 bước (MID position lệch), B sai edge case (CONTAINS vs STARTSWITH), D sai token_number. Không có đáp án "ngớ ngẩn".
3. **Solution giải thích 4/4 đáp án** — Đúng theo phong cách exam_style_analysis.
4. **Tất cả dùng chung dataset Superstore** — Nhất quán, sinh viên không bị phân tâm.

## Vấn đề phát hiện

### 🟡 WARNING — Câu 1: Choice B dùng CONTAINS có thể gây tranh cãi

**Vấn đề:** Solution nói `CONTAINS([Order ID], "US")` sai vì edge case `"CA-2021-1US966"`. Tuy nhiên, mã đơn hàng Superstore thực tế là **số thuần** (ví dụ `152156`), nên xác suất chứa "US" trong phần số là **gần như 0**. Sinh viên có thể argue rằng CONTAINS vẫn đúng trong practice.

**Đề xuất:** Solution đã giải thích tốt — nhấn mạnh STARTSWITH "chính xác hơn" chứ không nói CONTAINS "hoàn toàn sai". Giữ nguyên nhưng có thể thêm 1 câu: *"Trong thực tế, dù xác suất trùng thấp, STARTSWITH luôn là best practice khi chỉ cần check prefix."*

### 🟡 WARNING — Câu 3: Tooltip thực tế của Stacked Area

**Vấn đề:** Đề nói "hover vào đường biên trên cùng thấy tooltip hiện $120,000". Trong Tableau thực tế, khi hover vào Stacked Area, tooltip hiện **giá trị riêng** của vùng đang hover (ví dụ Slow = $30,000), KHÔNG phải tổng $120,000. Tổng chỉ hiện khi hover vào khoảng trống phía trên hoặc khi xem trục Y.

**Đề xuất:** Sửa đề thành: *"Bạn nhìn vào **trục Y** tại tháng đó và thấy đường biên trên cùng chạm khoảng **$120,000**"* — hoặc thêm chú thích: *"tooltip tổng được hiển thị qua Tooltip customization hoặc đọc từ trục Y"*.

### 🟢 MINOR — Câu 2: Nên ghi rõ DATEDIFF trả về gì khi NULL

**Vấn đề:** Đề yêu cầu sinh viên biết `DATEDIFF('day', [Order Date], NULL)` trả về NULL. Kiến thức này thuộc phần Date Functions trong tutorial, nhưng không phải tất cả sinh viên đều nhớ.

**Đề xuất:** Có thể thêm Lưu ý: *"(Lưu ý: Trong Tableau, bất kỳ phép tính nào có NULL đều trả về NULL, ví dụ `DATEDIFF('day', [Order Date], NULL)` = NULL.)"* — giúp sinh viên tập trung vào logic IF/ELSE thay vì đoán hành vi NULL.

### 🟢 MINOR — Câu 4: Liên kết với Câu 3 hơi yếu

**Vấn đề:** Câu 4 mở đầu "Sau khi phân tích tốc độ giao hàng ở Câu 3, bạn muốn xem xu hướng doanh thu..." — nhưng nội dung Câu 4 hoàn toàn không dùng `[Delivery Speed]` hay `[Order Region]` từ Câu 1-2. Liên kết chỉ ở mức câu mở đầu.

**Đề xuất:** Chấp nhận được — không phải mọi câu cần dùng trường đã tạo. Câu 4 test kiến thức Date trong Tableau (Discrete vs Continuous) là concept riêng biệt.

## Kết luận

Bộ đề đạt chất lượng tốt. Hai WARNING không ảnh hưởng đáp án (vẫn chỉ có 1 đáp án đúng mỗi câu) nhưng nên xem xét sửa Câu 3 về tooltip cho chính xác với hành vi Tableau thực tế.
