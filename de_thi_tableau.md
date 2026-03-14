# Đề thi — Tableau: Basic Charts & Preprocessing Functions

> **Độ khó:** Trung bình khó  
> **Số câu:** 4 câu trắc nghiệm (mỗi câu 4 đáp án A/B/C/D)  
> **Tài liệu gốc:** Basic Charts, Preprocessing Functions  
> **Dataset tham khảo:** Tableau Superstore

---

## Câu 1. (Preprocessing Functions — String Functions)

Trong dataset Superstore, cột `Order ID` có cấu trúc cố định: `"[Mã khu vực 2 ký tự]-[Năm đặt hàng 4 ký tự]-[Mã đơn hàng 6 ký tự]"`. Ví dụ: `"CA-2021-152156"`, `"US-2022-108966"`.

Bạn cần tạo 3 Calculated Fields trong Tableau:
- **Field 1:** Trích xuất mã khu vực (2 ký tự đầu) — ví dụ `"CA"`
- **Field 2:** Trích xuất năm đặt hàng (4 ký tự ở giữa) — ví dụ `"2021"`
- **Field 3:** Kiểm tra đơn hàng có phải khu vực "US" không — ví dụ `True` hoặc `False`

**(Lưu ý:** Trong Tableau, hàm `MID(string, start, length)` sử dụng chỉ mục **1-indexed** — ký tự đầu tiên ở vị trí 1.)

**Câu hỏi:** Bộ Calculated Fields nào sau đây cho kết quả **đúng** cho cả 3 yêu cầu trên?

**(A)**
```
Field 1: LEFT([Order ID], 2)
Field 2: MID([Order ID], 3, 4)
Field 3: STARTSWITH([Order ID], "US")
```

**(B)**
```
Field 1: SPLIT([Order ID], "-", 1)
Field 2: MID([Order ID], 4, 4)
Field 3: CONTAINS([Order ID], "US")
```

**(C)**
```
Field 1: SPLIT([Order ID], "-", 1)
Field 2: SPLIT([Order ID], "-", 2)
Field 3: STARTSWITH([Order ID], "US")
```

**(D)**
```
Field 1: SPLIT([Order ID], "-", 2)
Field 2: SPLIT([Order ID], "-", 3)
Field 3: STARTSWITH([Order ID], "US")
```

---

## Câu 2. (Preprocessing Functions — Date Functions & NULL Handling)

Tiếp tục với dataset Superstore, bạn cần tạo một Calculated Field `[Delivery Speed]` để phân loại tốc độ giao hàng dựa trên khoảng cách giữa `Order Date` và `Ship Date`. Quy tắc phân loại:
- **"Fast"** nếu giao trong vòng 2 ngày (≤ 2 ngày)
- **"Normal"** nếu giao trong 3–5 ngày
- **"Slow"** nếu giao trên 5 ngày

Ngoài ra, một số đơn hàng chưa có `Ship Date` (giá trị NULL). Với những đơn hàng này, `[Delivery Speed]` cần trả về `"Pending"` thay vì NULL.

**(Lưu ý:** Trong Tableau, bất kỳ phép tính nào có NULL đều trả về NULL — ví dụ `DATEDIFF('day', [Order Date], NULL)` = NULL, và `NULL <= 2` = NULL (không phải True hay False).)

*(Calculated Field `[Delivery Speed]` này sẽ được sử dụng trong Câu 3.)*

**Câu hỏi:** Công thức Calculated Field nào sau đây cho kết quả **đúng** trong mọi trường hợp?

**(A)**
```
IF DATEDIFF('day', [Order Date], [Ship Date]) <= 2 THEN "Fast"
ELSEIF DATEDIFF('day', [Order Date], [Ship Date]) <= 5 THEN "Normal"
ELSE "Slow"
END
```

**(B)**
```
IF ISNULL([Ship Date]) THEN "Pending"
ELSEIF DATEDIFF('day', [Order Date], [Ship Date]) <= 2 THEN "Fast"
ELSEIF DATEDIFF('day', [Order Date], [Ship Date]) <= 5 THEN "Normal"
ELSE "Slow"
END
```

**(C)**
```
IF ZN(DATEDIFF('day', [Order Date], [Ship Date])) <= 2 THEN "Fast"
ELSEIF ZN(DATEDIFF('day', [Order Date], [Ship Date])) <= 5 THEN "Normal"
ELSEIF ISNULL([Ship Date]) THEN "Pending"
ELSE "Slow"
END
```

**(D)**
```
IF DATEDIFF('day', [Order Date], [Ship Date]) <= 2 THEN "Fast"
ELSEIF DATEDIFF('day', [Order Date], [Ship Date]) <= 5 THEN "Normal"
ELSEIF ISNULL([Ship Date]) THEN "Pending"
ELSE "Slow"
END
```

---

## Câu 3. (Basic Charts — Stacked Area Chart)

Sử dụng Calculated Field `[Delivery Speed]` **đã tạo ở Câu 2** (giả sử bạn đã chọn công thức đúng), bạn muốn xem xu hướng doanh thu theo tốc độ giao hàng qua thời gian. Bạn filter bỏ các đơn hàng `"Pending"` và thực hiện các bước sau:

1. Kéo `Order Date` vào **Columns shelf**, nhấp chuột phải → chọn **Month** (Continuous — nhóm dưới, pill xanh lá)
2. Kéo `Sales` vào **Rows shelf**
3. Trong Marks card, chuyển **Mark Type** từ "Automatic" (đang là Line) sang **Area**
4. Kéo `[Delivery Speed]` vào ô **Color** trong Marks card

Biểu đồ hiện tại là **Stacked Area Chart** với 3 vùng màu (Fast, Normal, Slow) xếp chồng lên nhau. Tại một tháng cụ thể, bạn nhìn vào **trục Y** và thấy **đường biên trên cùng** (top edge) của toàn bộ biểu đồ chạm khoảng **$120,000**.

**Câu hỏi:** Phát biểu nào mô tả **đúng nhất** cách đọc biểu đồ Stacked Area này?

**(A)** Mỗi vùng màu hoạt động **độc lập** — đường biên trên (top edge) của mỗi vùng là giá trị `SUM(Sales)` riêng của nhóm Delivery Speed đó. Ba đường biên trên tương đương 3 đường trong Line Chart. Giá trị $120,000 là doanh thu riêng của nhóm nằm trên cùng.

**(B)** Các vùng **xếp chồng** lên nhau — đường biên trên cùng ($120,000) là **tổng doanh thu cả 3 nhóm Delivery Speed cộng lại** tại tháng đó. **Chiều cao** (thickness) của từng vùng màu mới thể hiện `SUM(Sales)` riêng của nhóm đó. Vùng trên cùng bị "đẩy lên" bởi các vùng bên dưới, nên đường biên trên của nó **không phải** giá trị riêng.

**(C)** Stacked Area Chart mặc định hiển thị **tỷ lệ phần trăm** — mỗi vùng thể hiện % đóng góp của nhóm Delivery Speed, tổng 3 vùng luôn = 100%. Giá trị $120,000 trên tooltip là tổng tuyệt đối, nhưng trục Y hiển thị 0–100%.

**(D)** Stacked Area Chart tự động sắp xếp các nhóm Delivery Speed sao cho nhóm có doanh thu lớn nhất luôn nằm ở **vùng dưới cùng**. Nếu thứ tự doanh thu thay đổi qua từng tháng, thứ tự các vùng cũng thay đổi theo để luôn giữ nhóm lớn nhất ở dưới.

---

## Câu 4. (Basic Charts — Discrete vs Continuous Date)

Sau khi phân tích tốc độ giao hàng ở Câu 3, bạn muốn xem xu hướng doanh thu theo thời gian bằng **Line Chart**. Bạn kéo `Order Date` vào Columns shelf và `Sales` vào Rows shelf, sau đó nhấp chuột phải vào pill `Order Date` trên Columns shelf.

Menu hiện ra với 2 nhóm tùy chọn (phân cách bởi đường kẻ ngang):
- **Nhóm trên** (Discrete — pill màu xanh dương): Year, Quarter, Month, Day...
- **Nhóm dưới** (Continuous — pill màu xanh lá): Year, Quarter, Month, Day...

Bạn chọn **`Month`** từ **nhóm trên (Discrete)**. Line Chart hiển thị 12 header: January, February, ..., December. Dữ liệu Superstore có 4 năm (2021–2024).

**Câu hỏi:** Phát biểu nào mô tả **đúng nhất** hành vi của biểu đồ khi chọn `Month` Discrete (nhóm trên)?

**(A)** Tableau tạo trục thời gian liên tục từ January 2021 đến December 2024, đường kẻ nối liền mạch qua 48 tháng. Chọn Discrete hay Continuous không ảnh hưởng đến cách hiển thị Line Chart.

**(B)** Tableau tạo 12 header riêng biệt (January → December). Dữ liệu của **cùng tháng từ tất cả các năm** được gộp lại — ví dụ header "March" chứa tổng `SUM(Sales)` của March 2021 + March 2022 + March 2023 + March 2024. Đường kẻ nối 12 điểm này cho thấy xu hướng theo mùa (seasonality) nhưng **không thể thấy xu hướng tăng trưởng qua các năm**.

**(C)** Tableau tạo 48 header riêng biệt — 12 tháng × 4 năm. Đường kẻ bị **đứt** tại mỗi ranh giới năm (December → January năm sau) vì Discrete Date không nối giữa các năm.

**(D)** Tableau tạo 12 header riêng biệt (January → December) nhưng vẽ **4 đường riêng biệt**, mỗi đường cho một năm (2021, 2022, 2023, 2024), tự động phân biệt bằng màu.

---

*Hết đề*
