# Solution — Tableau: Basic Charts & Preprocessing Functions

---

## Câu 1 — Đáp án: **(C)**

### Giải thích từng đáp án:

**Cấu trúc Order ID:** `"CA-2021-152156"` → ký tự theo vị trí (1-indexed trong Tableau):
- Vị trí 1–2: `"CA"` (mã khu vực)
- Vị trí 3: `"-"` (dấu gạch)
- Vị trí 4–7: `"2021"` (năm đặt hàng)
- Vị trí 8: `"-"` (dấu gạch)
- Vị trí 9–14: `"152156"` (mã đơn hàng)

**Choice A sai:**
- Field 1: `LEFT([Order ID], 2)` → `"CA"` ✅
- Field 2: `MID([Order ID], 3, 4)` → Bắt đầu tại vị trí 3 (ký tự `"-"`), lấy 4 ký tự → `"-202"` ❌ (lấy phải dấu gạch + 3 ký tự đầu của năm)
- Field 3: `STARTSWITH([Order ID], "US")` → ✅

→ **Field 2 sai** vì start position bị lệch 1 đơn vị. Đúng phải là `MID([Order ID], 4, 4)`.

**Choice B sai:**
- Field 1: `SPLIT([Order ID], "-", 1)` → Tách theo `"-"`, lấy phần 1 → `"CA"` ✅
- Field 2: `MID([Order ID], 4, 4)` → Vị trí 4 = `"2"`, lấy 4 ký tự → `"2021"` ✅
- Field 3: `CONTAINS([Order ID], "US")` → **Có vấn đề!** `CONTAINS` kiểm tra chuỗi con ở **bất kỳ vị trí nào** trong chuỗi. Nếu mã đơn hàng tình cờ chứa `"US"` ở phần khác (ví dụ: `"CA-2021-1US966"`), `CONTAINS` trả về `True` sai. `STARTSWITH` chính xác hơn vì chỉ kiểm tra **đầu chuỗi** — đảm bảo `"US"` nằm ở vị trí mã khu vực chứ không phải trùng ngẫu nhiên.

→ **Field 3 không chính xác** — có thể trả về kết quả sai trong trường hợp edge case. Dù xác suất trùng thấp trong thực tế, `STARTSWITH` luôn là best practice khi chỉ cần kiểm tra prefix.

**Choice C đúng:**
- Field 1: `SPLIT([Order ID], "-", 1)` → Tách theo `"-"`, lấy phần 1 → `"CA"` ✅
- Field 2: `SPLIT([Order ID], "-", 2)` → Lấy phần 2 → `"2021"` ✅
- Field 3: `STARTSWITH([Order ID], "US")` → Chỉ check đầu chuỗi → ✅

→ **Tất cả 3 field đều đúng**, và cách dùng `SPLIT` không phụ thuộc vị trí ký tự cố định — nếu format mã thay đổi (ví dụ mã khu vực 3 ký tự `"VNM-2021-152156"`), `SPLIT` vẫn hoạt động.

**Choice D sai:**
- Field 1: `SPLIT([Order ID], "-", 2)` → Lấy phần thứ **2** → `"2021"` ❌ (đây là năm, không phải mã khu vực)
- Field 2: `SPLIT([Order ID], "-", 3)` → Lấy phần thứ **3** → `"152156"` ❌ (đây là mã đơn hàng, không phải năm)
- Field 3: `STARTSWITH([Order ID], "US")` → ✅

→ **Field 1 và Field 2 đều sai** vì token_number bị lệch — lấy nhầm phần.

---

## Câu 2 — Đáp án: **(B)**

### Giải thích từng đáp án:

**Choice A sai:** Công thức này **không xử lý trường hợp `Ship Date` = NULL**. Khi `Ship Date` là NULL, `DATEDIFF('day', [Order Date], NULL)` trả về **NULL**. Trong Tableau, `NULL <= 2` trả về NULL (không phải True hay False), nên điều kiện IF đầu tiên không khớp, ELSEIF cũng không khớp, và kết quả rơi vào ELSE → trả về `"Slow"`. Đơn hàng chưa giao bị phân loại sai thành "Slow" thay vì "Pending".

**Choice B đúng:** Công thức kiểm tra NULL **trước tiên** bằng `ISNULL([Ship Date])`. Nếu Ship Date là NULL → trả về "Pending" ngay lập tức, không tính DATEDIFF. Nếu Ship Date có giá trị → tiếp tục kiểm tra DATEDIFF và phân loại Fast/Normal/Slow. Thứ tự kiểm tra NULL trước là quan trọng vì IF/ELSEIF đánh giá **lần lượt từ trên xuống** — điều kiện đầu tiên True sẽ được chọn.

**Choice C sai:** Có 2 vấn đề:
1. `ZN(DATEDIFF(...))` khi Ship Date = NULL → DATEDIFF trả về NULL → ZN thay bằng 0 → `0 <= 2` = True → trả về "Fast". Đơn hàng chưa giao bị phân loại sai thành "Fast"!
2. Dòng `ISNULL([Ship Date])` đặt **sau** 2 điều kiện DATEDIFF, nên với ZN biến NULL thành 0, kiểm tra ISNULL sẽ **không bao giờ được đến** (vì điều kiện `<= 2` đã match trước rồi).

**Choice D sai:** Tương tự Choice A nhưng có thêm dòng `ISNULL([Ship Date])` ở vị trí ELSEIF thứ 3. Tuy nhiên, dòng này **không bao giờ được thực thi** vì: khi Ship Date = NULL, `DATEDIFF` trả về NULL → `NULL <= 2` = NULL (falsy) → `NULL <= 5` = NULL (falsy) → rơi thẳng vào **ELSE** → "Slow", bỏ qua kiểm tra ISNULL. Thứ tự kiểm tra sai — ISNULL phải đứng **đầu tiên**, không phải cuối.

---

## Câu 3 — Đáp án: **(B)**

### Giải thích từng đáp án:

**Choice A sai:** Trong Stacked Area Chart, các vùng **không** hoạt động độc lập. Chúng xếp chồng lên nhau — vùng thứ 2 bắt đầu từ đường biên trên của vùng thứ 1, vùng thứ 3 bắt đầu từ đường biên trên của vùng thứ 2. Do đó, đường biên trên của mỗi vùng phía trên **không phải** giá trị riêng của nhóm Delivery Speed đó — nó là **tổng cộng dồn**. Chỉ **chiều cao** (thickness) của mỗi vùng mới phản ánh đúng `SUM(Sales)` riêng. Nếu muốn 3 đường độc lập, cần dùng **Line Chart** (giữ Mark Type = Line thay vì chuyển sang Area).

**Choice B đúng:** Đây là cách đọc chính xác Stacked Area Chart. Khi kéo `[Delivery Speed]` vào Color với Mark Type = Area, Tableau tạo biểu đồ xếp chồng: vùng đầu tiên (ví dụ Fast) bắt đầu từ trục X (y=0), vùng thứ hai (Normal) xếp **trên** vùng đầu, vùng thứ ba (Slow) xếp **trên cùng**. Nhìn trên trục Y, đường biên trên cùng chạm $120,000 = tổng `SUM(Sales)` cả 3 nhóm tại tháng đó. Đây chính là "cạm bẫy" phổ biến nhất khi đọc Stacked Area: người xem dễ nhầm đường biên trên cùng là doanh thu riêng của nhóm trên cùng, dẫn đến đánh giá sai kích thước thực.

**Choice C sai:** Stacked Area Chart mặc định hiển thị giá trị **tuyệt đối** (`SUM(Sales)` bằng đô-la), **không phải** phần trăm. Trục Y hiển thị giá trị $ thực tế, không phải 0–100%. Để có biểu đồ Stacked Area dạng phần trăm (tổng = 100%), cần thêm bước: nhấp chuột phải vào `SUM(Sales)` → **Quick Table Calculation** → **Percent of Total**. Khi chưa thực hiện bước này, biểu đồ luôn hiển thị giá trị tuyệt đối.

**Choice D sai:** Thứ tự nhóm Delivery Speed trong Stacked Area Chart được **cố định** theo legend — Tableau **không** tự động xếp lại thứ tự các vùng dựa trên giá trị doanh thu. Nhóm nào nằm ở vùng dưới cùng, giữa, hay trên cùng được xác định bởi thứ tự trong Color legend và **giữ nguyên xuyên suốt** toàn bộ trục thời gian. Nếu muốn thay đổi thứ tự, phải kéo thủ công trong legend.

---

## Câu 4 — Đáp án: **(B)**

### Giải thích từng đáp án:

**Choice A sai:** Discrete và Continuous tạo ra kết quả **rất khác nhau**. Discrete Date (pill xanh dương) tạo header riêng biệt cho mỗi giá trị, Continuous Date (pill xanh lá) tạo trục liên tục. Nói "không ảnh hưởng" là hoàn toàn sai.

**Choice B đúng:** Khi chọn `Month` Discrete (nhóm trên), Tableau tạo 12 header: January, February, ..., December. Tất cả dữ liệu của cùng tháng từ mọi năm được **gộp lại** — ví dụ cột "March" chứa `SUM(Sales)` của March 2021 + March 2022 + March 2023 + March 2024. Biểu đồ cho thấy xu hướng theo mùa (tháng nào bán tốt nhất) nhưng mất hoàn toàn thông tin về xu hướng tăng trưởng năm này qua năm khác. Nếu muốn thấy xu hướng qua các năm, cần chọn `Month` Continuous (nhóm dưới — pill xanh lá) để tạo trục liên tục từ Jan 2021 → Dec 2024.

**Choice C sai:** Discrete `Month` **không** tạo 48 header. Discrete `Month` chỉ tạo 12 header (January → December) và gộp dữ liệu các năm lại. Để tạo nhiều header riêng theo năm-tháng, cần thêm `YEAR` Discrete vào Columns shelf nữa — khi đó mới có thể thấy đường bị đứt giữa các năm. Mô tả "đường bị đứt tại ranh giới năm" chỉ đúng với trường hợp sử dụng cả Year lẫn Month Discrete đồng thời.

**Choice D sai:** Discrete `Month` tự nó **không** tách 4 đường theo năm. Để có 4 đường riêng theo năm, cần kéo thêm một Dimension (như `YEAR(Order Date)`) vào ô Color trong Marks card. Với chỉ `Month` Discrete trên Columns, Tableau gộp tất cả năm vào 12 điểm duy nhất.

---

*Hết Solution*
