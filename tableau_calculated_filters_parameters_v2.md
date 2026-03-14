# Phân tích nâng cao trong Tableau — Calculated Fields, Filters, Parameters

> **Dataset sử dụng trong ví dụ:** Tableau Superstore — bộ dữ liệu mẫu có sẵn trong Tableau Desktop, tại menu **Help → Sample Workbooks → Superstore**. Dataset gồm ~10.000 dòng đơn hàng bán lẻ với các trường về sản phẩm, khách hàng, doanh thu và lợi nhuận.

## I. Giới thiệu — Từ "nhìn dữ liệu" đến "hiểu dữ liệu"

Ở buổi trước, chúng ta đã nắm được cách tạo các biểu đồ cơ bản — Bar Chart, Line Chart, Scatter Plot, Text Table — và dùng Preprocessing Functions để làm sạch dữ liệu. Đó là những kỹ năng nền tảng cho phép bạn trình bày thông tin trực quan.

Nhưng một nhà phân tích dữ liệu giỏi không chỉ "nhìn" được dữ liệu. Họ còn phải "hiểu" được dữ liệu — biết đặt câu hỏi đúng, lọc ra thông tin cốt lõi, và tạo ra những chỉ số mới phục vụ mục tiêu cụ thể. Chẳng hạn: "Khách hàng nào mang lại lợi nhuận cao nhất?", "Doanh thu khu vực nào đang giảm?", hay "Nếu ngưỡng VIP thay đổi thì danh sách khách hàng sẽ khác thế nào?".

Trong buổi học này, chúng ta sẽ trang bị 3 công cụ mạnh mẽ nhất của Tableau để trả lời những câu hỏi đó:

- **Calculated Fields** — tạo chỉ số mới từ dữ liệu hiện có (ví dụ: tỷ suất lợi nhuận, phân loại khách hàng)
- **Filters** — lọc dữ liệu để tập trung vào phần quan trọng (ví dụ: chỉ xem năm 2024, chỉ xem khách VIP)
- **Parameters** — tạo dashboard tương tác, cho phép người dùng tự chọn cách nhìn dữ liệu mà không cần viết code

Ba công cụ này bổ trợ cho nhau: Calculated Fields tạo ra chỉ số, Filters lọc theo chỉ số đó, và Parameters cho phép người dùng thay đổi tiêu chí lọc một cách linh hoạt. Kết hợp cả ba, bạn sẽ xây dựng được những dashboard chuyên nghiệp mà bất kỳ ai cũng có thể sử dụng.

Hãy bắt đầu với Calculated Fields — công cụ mà hầu như mọi dashboard chuyên nghiệp đều sử dụng.

## II. Calculated Fields nâng cao — Tạo chỉ số mới từ dữ liệu

### A. Tổng quan về Calculated Fields

**Calculated Fields** (trường tính toán) là cách tạo cột dữ liệu mới bằng cách viết biểu thức trực tiếp trong Tableau. Ở buổi trước, chúng ta đã dùng Calculated Fields cho các tác vụ **preprocessing** — chuyển đổi kiểu dữ liệu, làm sạch chuỗi, xử lý ngày tháng. Nhưng sức mạnh thực sự của Calculated Fields nằm ở khả năng tạo ra những **chỉ số phân tích** (analytics metrics) hoàn toàn mới — những thứ không tồn tại trong dữ liệu gốc nhưng cực kỳ có giá trị cho việc ra quyết định kinh doanh.

Tableau hỗ trợ **2 cách tạo** Calculated Field:

**Cách 1 — Dùng Calculated Field Editor (phổ biến nhất):**

1. Click chuột phải vào vùng trống trong **Data Pane** → chọn **Create Calculated Field...**
2. Hoặc vào menu **Analysis** → **Create Calculated Field**
3. Viết biểu thức trong cửa sổ Editor → nhấn **OK**
4. Field mới xuất hiện trong Data Pane, sẵn sàng để kéo vào view

**Cách 2 — Ad-hoc Calculation (tính nhanh, không lưu):**

Nếu chỉ cần tính nhanh một biểu thức đơn giản, bạn có thể nhập trực tiếp trên **Columns**, **Rows**, hoặc thẻ **Marks** mà không cần mở Editor. Ví dụ: double-click vào Columns shelf → gõ `SUM([Profit])/SUM([Sales])` → nhấn Enter. Tableau sẽ hiện kết quả ngay lập tức. Nếu muốn lưu lại Ad-hoc calculation, kéo nó vào Data Pane và đặt tên.

**Cách đọc Formula Editor:**

Khi viết công thức trong Calculated Field Editor, chú ý các **màu sắc** — chúng giúp bạn phát hiện lỗi nhanh:

| Màu / Ký hiệu | Ý nghĩa |
|:---------------|:--------|
| **Dòng chữ màu đỏ** | Lỗi cú pháp — di chuột qua để xem hướng dẫn sửa |
| **// văn bản màu xám** | Comment — không ảnh hưởng tính toán |
| **[text màu cam]** | Tên trường dữ liệu |
| **text màu xanh ()** | Tên hàm (function) |
| **[text màu tím]** | Tên parameter |
| **text in đậm** | Phép tính aggregate (tính cùng kết quả tổng hợp) |

Khi Editor hiện "The calculation contains errors" ở góc dưới, nghĩa là công thức có lỗi — kiểm tra dòng đỏ để sửa. Khi hiện "The calculation is valid", bạn đã sẵn sàng nhấn OK.

### B. Row-level vs Aggregate — Hai cấp độ tính toán

Tableau phân biệt rõ 2 cấp độ tính toán trong Calculated Fields. Hiểu sai sẽ cho kết quả sai hoàn toàn, nên chúng ta sẽ đi qua ví dụ cụ thể.

Giả sử dataset Superstore có 4 dòng đơn hàng *(dữ liệu minh họa, không phải bản ghi thật từ Superstore)*:

| Dòng | Region | Sales | Discount |
|:----:|:-------|------:|---------:|
| 1 | East | 200 | 0.1 |
| 2 | East | 300 | 0.2 |
| 3 | West | 500 | 0.15 |
| 4 | West | 100 | 0.05 |

**Row-level (cấp dòng)** — tính trên **từng dòng riêng lẻ**, trước khi Tableau gộp nhóm:

Ví dụ: tạo Calculated Field `[Sales] * [Discount]` (giảm giá mỗi đơn):

| Dòng | Region | Sales | Discount | **Sales * Discount** |
|:----:|:-------|------:|---------:|--------------------:|
| 1 | East | 200 | 0.1 | **20** |
| 2 | East | 300 | 0.2 | **60** |
| 3 | West | 500 | 0.15 | **75** |
| 4 | West | 100 | 0.05 | **5** |

Mỗi dòng tự tính cho mình — dòng 1 chỉ cần Sales=200 và Discount=0.1 của chính nó, không cần biết dòng khác. Đây là **row-level**.

**Aggregate (cấp nhóm)** — tính trên **nhóm dòng**, sau khi Tableau gộp theo dimensions trong view:

Ví dụ: tạo Calculated Field `SUM([Sales]) / COUNT([Sales])` (doanh thu trung bình). Nếu view có Region trên Rows, Tableau gộp thành 2 nhóm rồi mới tính:

| Region | Các dòng thuộc nhóm | **SUM(Sales) / COUNT(Sales)** |
|:-------|:-------------------|-----------------------------:|
| East | dòng 1 + dòng 2 | (200+300) / 2 = **250** |
| West | dòng 3 + dòng 4 | (500+100) / 2 = **300** |

Kết quả phụ thuộc vào **dimensions trong view**: nếu bỏ Region ra khỏi view, Tableau gộp cả 4 dòng thành 1 nhóm → kết quả: (200+300+500+100) / 4 = 275. Nếu thêm Sub-Category vào view, Tableau chia thành nhiều nhóm nhỏ hơn → kết quả khác nữa.

**Tóm lại:**

| | Row-level | Aggregate |
|:--|:----------|:----------|
| **Tính khi nào** | Trước khi gộp nhóm | Sau khi gộp nhóm |
| **Cần gì** | Chỉ thông tin từ 1 dòng | Thông tin từ nhiều dòng |
| **Kết quả thay đổi khi đổi view?** | Không — giá trị mỗi dòng cố định | Có — phụ thuộc dimensions trong view |
| **Nhận biết** | Không có hàm `SUM`, `AVG`... | Có hàm `SUM()`, `AVG()`, `COUNT()`... |
| **Ví dụ** | `[Sales] * 0.1`, `DATEDIFF(...)` | `SUM([Sales])`, `AVG([Profit])` |

**Lỗi thường gặp — "Cannot mix aggregate and non-aggregate":**

Khi bạn trộn lẫn 2 cấp trong cùng công thức, Tableau không biết xử lý thế nào và báo lỗi. Ví dụ:

```
// ❌ Lỗi: SUM là aggregate, [Discount] là row-level
SUM([Sales]) * [Discount]
```

Tableau không thể nhân tổng doanh thu cả nhóm (1 giá trị) với Discount từng dòng (nhiều giá trị). Giải pháp — đưa về cùng cấp:

```
// ✅ Cách 1: cả hai đều aggregate
SUM([Sales]) * AVG([Discount])

// ✅ Cách 2: cả hai đều row-level
[Sales] * [Discount]
```

Cách 1 cho kết quả "tổng doanh thu × discount trung bình" (1 giá trị cho mỗi nhóm). Cách 2 cho "doanh thu × discount từng dòng" (1 giá trị cho mỗi dòng, Tableau tự SUM khi hiển thị).

> **💡 Tip — Nhận biết cấp độ trên view**
>
> Khi kéo Calculated Field vào view, nhìn vào pill trên shelf: nếu pill hiển thị `SUM(...)` hoặc `AVG(...)` tức là Tableau đang aggregate. Nếu hiển thị tên field trực tiếp (không có hàm bao quanh) tức là row-level. Bạn cũng có thể click vào pill → chọn **Measure** và thay đổi kiểu aggregate mặc định.

### C. Các hàm Aggregation thường dùng

Khi viết Calculated Fields ở cấp aggregate, bạn sẽ dùng các hàm tổng hợp. Bảng dưới đây tóm tắt những hàm phổ biến nhất cùng ví dụ thực tế trên dataset Superstore:

| Hàm | Ý nghĩa | Ví dụ | Khi nào dùng |
|:-----|:---------|:------|:-------------|
| `SUM()` | Tổng tất cả giá trị | `SUM([Sales])` → tổng doanh thu | So sánh tổng giữa các nhóm |
| `AVG()` | Trung bình cộng | `AVG([Profit])` → lợi nhuận trung bình | Đánh giá mức trung bình |
| `MIN()` | Giá trị nhỏ nhất | `MIN([Order Date])` → ngày đặt hàng đầu tiên | Tìm giá trị cực tiểu |
| `MAX()` | Giá trị lớn nhất | `MAX([Order Date])` → ngày đặt hàng gần nhất | Tìm giá trị cực đại |
| `COUNT()` | Đếm số dòng | `COUNT([Order ID])` → tổng số đơn | Đếm bản ghi (bao gồm trùng lặp) |
| `COUNTD()` | Đếm giá trị duy nhất | `COUNTD([Customer ID])` → số khách hàng | Đếm bản ghi không trùng |

> **⚠️ Lưu ý — COUNTD chậm với dữ liệu lớn**
>
> Hàm `COUNTD()` (Count Distinct) phải quét và so sánh từng giá trị để loại trùng, nên tốn tài nguyên hơn `COUNT()` đáng kể. Với dataset lớn (hàng triệu dòng), nên cân nhắc dùng `COUNT()` kết hợp với cách tổ chức dữ liệu hợp lý thay vì dùng `COUNTD()` ở nhiều nơi.

### D. Ví dụ thực tế — Tạo chỉ số kinh doanh

Chúng ta sẽ tạo một số chỉ số quan trọng từ dataset Superstore. Mỗi ví dụ đều đi kèm giải thích chi tiết:

**1. Tỷ suất lợi nhuận (Profit Margin)**

Tỷ suất lợi nhuận cho biết cứ mỗi đồng doanh thu, bạn giữ lại bao nhiêu đồng lợi nhuận. Đây là chỉ số then chốt để đánh giá hiệu quả kinh doanh:

```
// Tỷ suất lợi nhuận = Lợi nhuận / Doanh thu
// ZN() chuyển NULL thành 0 — tránh lỗi chia cho NULL
ZN(SUM([Profit])) / ZN(SUM([Sales]))
```

Sau khi tạo, kéo field này vào view và format thành **Percentage** (click chuột phải vào pill → **Format** → **Numbers** → **Percentage**). Giá trị 0.25 nghĩa là cứ 100 đồng doanh thu, bạn lãi 25 đồng.

**2. Doanh thu trung bình mỗi đơn hàng (Average Order Value)**

Chỉ số này giúp đánh giá giá trị trung bình mỗi giao dịch — doanh thu cao có thể do nhiều đơn nhỏ hoặc ít đơn lớn:

```
// Tổng doanh thu / Tổng số đơn hàng
SUM([Sales]) / COUNTD([Order ID])
```

Lưu ý: dùng `COUNTD` thay vì `COUNT` vì mỗi Order ID có thể xuất hiện nhiều lần (một đơn hàng chứa nhiều sản phẩm). `COUNTD` chỉ đếm mỗi đơn một lần.

**3. Số ngày giao hàng (Delivery Days)**

Chỉ số này là **row-level** vì chỉ cần thông tin từ một dòng (ngày đặt + ngày giao):

```
// Số ngày từ lúc đặt đến lúc giao
DATEDIFF('day', [Order Date], [Ship Date])
```

`DATEDIFF` nhận 3 tham số: đơn vị thời gian ('day', 'month', 'year'...), ngày bắt đầu, ngày kết thúc. Kết quả là số nguyên — ví dụ 3 nghĩa là giao sau 3 ngày.

Hàm ngược lại của `DATEDIFF` là **`DATEADD`** — cộng thêm khoảng thời gian vào một ngày:

```
// Ngày dự kiến giao hàng = Order Date + 5 ngày
DATEADD('day', 5, [Order Date])
```

`DATEADD` nhận 3 tham số: `DATEADD('đơn vị', số_lượng, trường_ngày)`. Ví dụ: `DATEADD('month', 3, [Order Date])` cho ra ngày sau 3 tháng. Hai hàm này là cặp đôi không thể thiếu khi phân tích dữ liệu theo thời gian.

**4. Phân loại khách hàng theo doanh thu (Customer Tier)**

Phân loại khách hàng thành các nhóm giúp tập trung nguồn lực vào nhóm quan trọng nhất:

```
// Phân loại dựa trên tổng doanh thu mỗi khách hàng
IF SUM([Sales]) > 10000 THEN "VIP"
ELSEIF SUM([Sales]) > 5000 THEN "Regular"
ELSE "New"
END
```

Field này là **aggregate** (dùng `SUM`), nên kết quả phụ thuộc vào dimensions trong view. Nếu view có Customer Name, mỗi khách hàng sẽ được phân loại riêng. Nếu view có Region, phân loại sẽ dựa trên tổng doanh thu cả region.

> **💡 Tip — Dùng CASE thay IF khi logic đơn giản**
>
> Khi so sánh một field với nhiều giá trị cố định, `CASE` thường nhanh hơn chuỗi `IF/ELSEIF` dài. Ví dụ: `CASE [Ship Mode] WHEN "First Class" THEN 1 WHEN "Second Class" THEN 2 ELSE 3 END`. Tableau optimize `CASE` tốt hơn vì nó biết trước đang so sánh cùng một field.

### E. LOD Expressions — Tính toán ở mức chi tiết khác

**LOD (Level of Detail) Expressions** là tính năng nâng cao nhưng cực kỳ mạnh mẽ — và cũng là chủ đề phức tạp nhất trong buổi học này. Đừng lo nếu chưa hiểu ngay lần đầu — chúng ta sẽ bắt đầu với loại phổ biến nhất (FIXED) rồi mới đến INCLUDE và EXCLUDE.

Bình thường, khi bạn viết `SUM([Sales])`, Tableau tự động tính tổng theo các dimensions có trong view (ví dụ: nếu view có Region, Tableau tính SUM cho từng Region). LOD cho phép bạn **bỏ qua** mức chi tiết đang hiển thị và tính ở mức bạn muốn.

Ví dụ đơn giản: bạn muốn biết tổng doanh thu của **mỗi khách hàng**, bất kể view đang hiển thị theo Region hay Category. Với cách thông thường, bạn phải kéo Customer Name vào view. Với LOD, chỉ cần viết:

```
{ FIXED [Customer Name] : SUM([Sales]) }
```

Công thức này nói: "Tính tổng doanh thu, nhóm theo Customer Name, bất kể view đang hiển thị gì."

Tableau có **3 loại LOD**, mỗi loại phục vụ mục đích khác nhau:

| Loại | Cú pháp | Ý nghĩa | Ví dụ |
|:-----|:--------|:---------|:------|
| **FIXED** | `{ FIXED [Dim] : AGG() }` | Tính theo dimensions **chỉ định**, bỏ qua mọi thứ khác trong view | `{ FIXED [Region] : SUM([Sales]) }` → doanh thu mỗi region, bất kể view |
| **INCLUDE** | `{ INCLUDE [Dim] : AGG() }` | **Thêm** dimensions vào mức hiện tại — kết quả chi tiết hơn view | `{ INCLUDE [Category] : AVG([Profit]) }` → lợi nhuận trung bình theo category + dimensions trong view |
| **EXCLUDE** | `{ EXCLUDE [Dim] : AGG() }` | **Loại bỏ** dimensions khỏi mức hiện tại — kết quả tổng quát hơn view | `{ EXCLUDE [Region] : SUM([Sales]) }` → tổng doanh thu không phân theo region |

**Ví dụ so sánh trực quan:**

Giả sử view đang hiển thị doanh thu theo **Category** (Furniture, Office Supplies, Technology):

| Công thức | Furniture | Office Supplies | Technology | Giải thích |
|:----------|----------:|----------------:|-----------:|:-----------|
| `SUM([Sales])` | 741K | 719K | 836K | Tính theo từng Category |
| `{ FIXED : SUM([Sales]) }` | 2,297K | 2,297K | 2,297K | Tổng toàn bộ, không phân nhóm |
| `{ EXCLUDE [Category] : SUM([Sales]) }` | 2,297K | 2,297K | 2,297K | Bỏ qua Category → tương tự FIXED không dimension |

Khi nào dùng loại nào? Nếu bạn cần một giá trị cố định để so sánh (như "tổng doanh thu toàn công ty"), dùng **FIXED**. Nếu cần tính chi tiết hơn view hiện tại (như "doanh thu từng sản phẩm" khi view chỉ hiện Category), dùng **INCLUDE**. Nếu cần bỏ qua một dimension trong view (như "tổng không phân theo quý" khi view có cả Quarter), dùng **EXCLUDE**.

> **💡 Tip — Tạo LOD nhanh bằng Ctrl+Drag**
>
> Từ phiên bản mới, bạn có thể tạo FIXED LOD nhanh mà không cần mở Editor: giữ phím **Ctrl** rồi kéo một measure lên trên một dimension. Ví dụ: giữ Ctrl, kéo **Balance** thả vào **Portfolio** → Tableau tự tạo calculated field `{ FIXED [Portfolio] : SUM([Balance]) }`. Thao tác này tiết kiệm thời gian đáng kể khi cần tạo nhiều LOD.

> **⚠️ Lưu ý — FIXED LOD và Filters**
>
> FIXED LOD được tính toán **trước** Dimension Filters trong Order of Operations của Tableau. Nghĩa là nếu bạn lọc Region = "East", FIXED LOD vẫn tính trên **toàn bộ** dữ liệu (bao gồm cả West, Central, South). Muốn FIXED LOD bị ảnh hưởng bởi filter, bạn cần đặt filter đó làm **Context Filter** (sẽ học ở phần III).

### F. Order of Operations — Thứ tự Tableau xử lý dữ liệu

Khi bạn xây dựng view với nhiều Calculated Fields, Filters, và LOD, Tableau thực hiện mọi thứ theo một **thứ tự cố định**. Hiểu thứ tự này giúp bạn tránh kết quả sai và debug hiệu quả.

Thứ tự từ sớm nhất đến muộn nhất (theo Tableau official documentation):

```
1. [Extract Filters]         — lọc khi tạo extract
2. [Data Source Filters]     — lọc tại nguồn dữ liệu
3. [Context Filters]         — tạo "bối cảnh" cho các filter sau
4. [Sets, FIXED LOD, Top N]  — Sets, FIXED LOD tính ở đây
5. [Dimension Filters]       — lọc theo categories
6. [INCLUDE/EXCLUDE LOD]     — INCLUDE, EXCLUDE tính ở đây
7. [Measure Filters]         — lọc theo giá trị số (SUM > X)
8. [Table Calculations]      — Running Total, Rank, Percent...
9. [Table Calc Filters]      — lọc trên kết quả Table Calculations
```

**Ý nghĩa thực tế:**
- **FIXED LOD** (bước 4) nằm **trước** Dimension Filters (bước 5) → filter thông thường không ảnh hưởng FIXED
- **INCLUDE/EXCLUDE** (bước 6) nằm **sau** Dimension Filters (bước 5) → filter thông thường ảnh hưởng INCLUDE/EXCLUDE
- **Context Filters** (bước 3) nằm **trước** FIXED LOD (bước 4) → Context Filter ảnh hưởng cả FIXED LOD

Đây là lý do Context Filters quan trọng — chúng "đẩy" filter lên trước các phép tính LOD. Ta sẽ tìm hiểu chi tiết ở phần Filters.

### G. Quick Table Calculations — Tính toán nhanh trên dữ liệu đã hiển thị

**Quick Table Calculations** là tính năng cho phép thực hiện các phép tính phổ biến mà không cần viết công thức thủ công. Điểm khác biệt quan trọng: Quick Table Calculations tính trên **dữ liệu đã được hiển thị** trong view (sau tất cả các bước filter), không phải trên toàn bộ dataset.

**Về kiến trúc:** Row-level và aggregate calculations được gửi đến data source để xử lý (qua SQL, MDX, hoặc TQL). Table calculations thì khác — chúng được thực hiện trên **bộ nhớ cache** của Tableau, sau khi dữ liệu đã được tổng hợp và trả về. Đây là lý do chúng nằm cuối cùng trong Order of Operations.

Bảng dưới đây tóm tắt các loại Quick Table Calculations thường dùng:

| Loại | Mô tả | Ví dụ thực tế |
|:-----|:------|:--------------|
| **Running Total** | Cộng dồn từ đầu đến vị trí hiện tại | Doanh thu tích lũy theo tháng: T1=100, T2=100+150=250, T3=250+200=450 |
| **Difference** | Chênh lệch giá trị so với giá trị trước | Tháng 2 bán nhiều hơn tháng 1: 150-100=50 |
| **Percent Difference** | Chênh lệch % so với kỳ trước | Doanh thu tháng 3 tăng 15% so với tháng 2 |
| **Percent of Total** | Phần trăm so với tổng | Q1 chiếm 28%, Q2 chiếm 32% trong tổng năm |
| **Rank** | Xếp hạng | Sản phẩm A xếp hạng 1 về doanh thu |
| **Percentile** | Phần trăm thống kê của giá trị | Khách hàng A nằm ở percentile 95 về doanh thu |
| **Moving Average** | Trung bình động — làm mịn biến động ngắn hạn | Trung bình doanh thu 3 tháng gần nhất |
| **YTD Total** | Tổng từ đầu năm đến hiện tại | Doanh thu từ tháng 1 đến tháng hiện tại |
| **Compound Growth Rate** | Tỷ lệ tăng trưởng kép | Giá trị hiện tại theo phần trăm từ giá trị đầu tiên |
| **Year Over Year Growth** | So sánh với cùng kỳ năm trước | Doanh thu Q2/2024 so với Q2/2023 |

**Cách tạo Quick Table Calculations:**

1. Kéo measure (ví dụ `Sales`) vào view
2. Click chuột phải vào pill `SUM(Sales)` trên shelf
3. Chọn **Quick Table Calculation** → chọn loại (ví dụ Running Total)
4. Pill sẽ hiện thêm ký hiệu tam giác (△) biểu thị Table Calculation

**4 mức kiểm soát Table Calculation:**

Tableau cung cấp 4 mức kiểm soát từ đơn giản đến phức tạp:

| Mức | Mô tả |
|:----|:------|
| **Quick Table Calculation** | Chọn từ menu — tự động thiết lập measure, phạm vi, hướng mặc định |
| **Add Table Calculation** | Kiểm soát hơn — chọn measure và loại tính toán từ menu |
| **Edit Table Calculation** | Mở khi table calc đã được áp dụng — cho phép thay đổi hướng và phạm vi |
| **Custom Table Calculation** | Viết công thức tùy chỉnh trong Calculated Field Editor — linh hoạt nhất |

**Scope, Direction, và Compute Using:**

Sau khi tạo Table Calculation, bạn cần xác định **phạm vi (scope)** và **hướng (direction)** — tức là Tableau sẽ tính trên vùng nào và theo hướng nào:

- **Scope** (phạm vi) xác định ranh giới tính toán:
  - **Table**: toàn bộ bảng — Running Total cộng dồn qua tất cả dòng/cột
  - **Pane**: từng ô lớn — nếu view chia theo Region, mỗi Region tính riêng
  - **Cell**: từng ô nhỏ nhất — mỗi giao điểm dòng-cột là một nhóm riêng

- **Direction** (hướng) xác định thứ tự tính:
  - **Across**: từ trái sang phải
  - **Down**: từ trên xuống dưới
  - **Across then Down**: trái→phải rồi xuống dòng tiếp
  - **Down then Across**: trên→dưới rồi sang cột tiếp

- **Specific Dimension**: thay vì chọn scope/direction, bạn có thể chỉ định dimension cụ thể — ví dụ tính Running Total theo `Order Date` (theo tháng)

> **💡 Tip — Relative To**
>
> Một số Quick Table Calculations cho phép thay đổi **Relative To** — tức là so sánh với giá trị nào. Ví dụ: Percent Difference mặc định so với **Previous** (kỳ trước), nhưng bạn có thể chuyển sang **First** (kỳ đầu tiên) để tính tăng trưởng tích lũy. Click chuột phải vào pill → **Relative To** → chọn loại.

Chọn sai Compute Using sẽ cho kết quả không mong đợi. Ví dụ: Running Total theo tháng nhưng Compute Using đang là "Table (across)" sẽ cộng dồn theo cột thay vì theo dòng. Luôn kiểm tra bằng cách hover vào các giá trị trên chart để xác nhận kết quả đúng.

**So sánh Quick Table Calculations vs Calculated Fields:**

| Tiêu chí | Quick Table Calculations | Calculated Fields |
|:----------|:------------------------|:-----------------|
| **Cách tạo** | Click chuột phải, chọn menu — không cần viết code | Viết biểu thức thủ công |
| **Tính trên** | Dữ liệu **đã hiển thị** trong view (cache) | Toàn bộ dataset (hoặc theo LOD) |
| **Nơi xử lý** | Trên máy chạy Tableau (cache) | Trên data source (SQL) |
| **Độ linh hoạt** | Chỉ các loại có sẵn (10 loại) | Hoàn toàn linh hoạt — viết logic bất kỳ |
| **Hiệu năng** | Có thể chậm với dữ liệu lớn (tính sau cùng) | Thường nhanh hơn (tính sớm hơn trong pipeline) |

## III. Filters — Lọc dữ liệu để tập trung phân tích

### A. Tại sao cần Filters?

Khi làm việc với dataset có hàng chục nghìn dòng như Superstore, hiển thị tất cả dữ liệu cùng lúc sẽ tạo biểu đồ rối mắt, chậm, và khó rút ra insight. Filters giúp bạn "cắt" dữ liệu — chỉ giữ lại phần quan trọng. Ví dụ: chỉ xem doanh thu năm 2024, chỉ xem khách hàng VIP, hoặc chỉ xem sản phẩm có lợi nhuận dương.

Nhưng filters trong Tableau không đơn giản chỉ là "ẩn dữ liệu". Mỗi loại filter hoạt động ở **giai đoạn khác nhau** trong Order of Operations, ảnh hưởng đến hiệu năng và kết quả theo cách khác nhau. Hiểu đúng các loại filter giúp bạn tránh kết quả sai và tối ưu tốc độ dashboard.

### B. Các loại Filters trong Tableau

Tableau có **6 loại filters chính**, xếp theo thứ tự thực hiện từ sớm đến muộn:

| # | Loại Filter | Thời điểm | Mô tả | Tương đương SQL |
|:-:|:------------|:----------|:------|:----------------|
| 1 | **Extract Filters** | Khi tạo extract | Lọc ngay lúc import dữ liệu — dữ liệu bị loại **không tồn tại** trong extract | Giống `WHERE` lúc copy data |
| 2 | **Data Source Filters** | Trước khi vào worksheet | Lọc toàn bộ workbook — tất cả sheets đều bị ảnh hưởng | Giống `WHERE` trên table |
| 3 | **Context Filters** | Trước dimension filters | Tạo "bối cảnh" — subset dữ liệu mà các filter khác hoạt động trên đó | Giống tạo bảng tạm `WITH` |
| 4 | **Dimension Filters** | Sau context, trước measure | Lọc theo categories (Region, Category, Ship Mode...) | Giống `WHERE category IN (...)` |
| 5 | **Measure Filters** | Sau dimension | Lọc theo giá trị số đã aggregate (SUM(Sales) > 1000) | Giống `HAVING` |
| 6 | **Table Calc Filters** | Cuối cùng | Lọc trên kết quả Table Calculations — chỉ **ẩn** dữ liệu, không loại bỏ khỏi tính toán | Không có tương đương |

Điểm then chốt: filters ở **vị trí sớm hơn** sẽ loại bỏ dữ liệu trước khi Tableau tính toán → hiệu năng tốt hơn. Extract và Data Source Filters nhanh nhất vì loại dữ liệu ngay từ đầu. Table Calc Filters chậm nhất vì phải tính xong mọi thứ rồi mới lọc.

### C. Cách tạo Filter

Có 2 cách tạo filter phổ biến:

**Cách 1 — Kéo thả (dùng nhiều nhất):**

1. Trong **Data Pane**, kéo field bạn muốn lọc vào khung **Filters** (nằm ở bên trái view)
2. Tableau hiện hộp thoại filter tùy thuộc loại field:
   - Dimension → hộp thoại danh sách checkbox (chọn/bỏ giá trị)
   - Measure → hộp thoại chọn kiểu aggregation + range
3. Chọn giá trị cần lọc → **OK**

**Cách 2 — Click chuột phải (hiện filter control):**

1. Click chuột phải vào field trong **Data Pane** hoặc trên view
2. Chọn **Show Filter**
3. Filter hiện dưới dạng control (dropdown, slider, checkbox...) ở bên phải view
4. Người dùng có thể tự chọn giá trị lọc

### D. Dimension Filters — Lọc theo phân loại

**Dimension Filters** lọc theo các giá trị phân loại — Region, Category, Customer Name, Ship Mode... Khi kéo dimension vào Filters shelf, Tableau hiện hộp thoại với 4 tab:

| Tab | Mô tả | Ví dụ |
|:----|:------|:------|
| **General** | Chọn/bỏ giá trị cụ thể từ danh sách | Chọn East và West, bỏ Central và South |
| **Wildcard** | Lọc theo mẫu text | Khách hàng có tên bắt đầu bằng "John" |
| **Condition** | Lọc theo điều kiện logic | Các category có SUM(Sales) > 500,000 |
| **Top** | Lọc top/bottom N theo measure | Top 10 sản phẩm theo doanh thu |

**Ví dụ: lọc chỉ hiển thị East và West:**

1. Kéo **Region** vào Filters shelf
2. Ở tab **General**, bỏ check Central và South
3. OK → view chỉ còn East và West

Bạn cũng có thể tạo Calculated Field làm dimension filter linh hoạt hơn:

```
// Tạo Calculated Field boolean
[Region] = "East" OR [Region] = "West"
```

Kéo field này vào Filters → chọn **True** → kết quả tương tự.

### E. Measure Filters — Lọc theo giá trị số

**Measure Filters** lọc dựa trên giá trị số **đã aggregate** — doanh thu, lợi nhuận, số lượng... Khi kéo measure vào Filters, Tableau yêu cầu bạn chọn kiểu aggregation trước (SUM, AVG, MIN, MAX...), rồi mới chọn cách lọc.

**Ví dụ: chỉ hiển thị sub-category có doanh thu trên 100,000:**

1. Kéo **Sales** vào Filters shelf
2. Chọn **Sum** làm kiểu aggregation
3. Chọn **At least** → nhập 100,000
4. OK → chỉ các sub-category có tổng doanh thu ≥ 100,000 mới hiển thị

Các tùy chọn lọc measure:
- **Range of values**: giá trị từ X đến Y
- **At least**: giá trị ≥ X
- **At most**: giá trị ≤ X
- **Special**: xử lý NULL values (bao gồm / loại trừ NULL)

### F. Context Filters — Quan trọng nhưng hay bị hiểu sai

**Context Filter** là loại filter đặc biệt. Khi bạn đánh dấu một filter là "Add to Context", nó sẽ chạy **trước** tất cả dimension filters, measure filters, và FIXED LOD — tạo ra một subset dữ liệu "cô lập" mà các filter khác hoạt động trên đó.

**Ví dụ kinh điển — Top 10 trong năm 2024:**

Bạn muốn tìm top 10 sản phẩm có doanh thu cao nhất **trong năm 2024**. Nếu làm theo cách thông thường:

1. Kéo **Year([Order Date])** vào Filters → chọn 2024
2. Kéo **Product Name** vào Filters → tab **Top** → chọn Top 10 by SUM(Sales)

**Vấn đề**: Tableau tính top 10 toàn bộ rồi mới lọc năm 2024 (vì cả hai đều là dimension filters, chạy cùng bước). Kết quả sai — bạn có thể thấy ít hơn 10 sản phẩm vì một số sản phẩm top không bán trong 2024.

**Giải pháp**: Đặt Year filter làm Context Filter:

1. Click chuột phải vào filter **Year** trong Filters shelf
2. Chọn **Add to Context**
3. Filter chuyển sang **màu xám đậm** (thay vì màu bình thường)

Bây giờ Tableau lọc năm 2024 **trước** (bước 3 trong pipeline), rồi mới tính top 10 (bước 4). Kết quả đúng — top 10 sản phẩm của riêng năm 2024.

> **💡 Tip — Khi nào dùng Context Filter?**
>
> Dùng Context Filter khi bạn cần một filter chạy **trước** các filter khác. Hai trường hợp phổ biến nhất: (1) Top N kết hợp với filter khác, (2) muốn FIXED LOD bị ảnh hưởng bởi filter (vì FIXED nằm sau Context nhưng trước Dimension).

### G. Wildcard Filter — Lọc theo mẫu text

Khi cần lọc theo một phần của chuỗi (không phải giá trị chính xác), dùng **Wildcard Filter**. Đây là tính năng rất hữu ích khi dữ liệu có tên dài hoặc không đồng nhất.

**Ví dụ: lọc tất cả sản phẩm có từ "Chair" trong tên:**

1. Kéo **Product Name** vào Filters
2. Chọn tab **Wildcard**
3. Nhập "Chair" và chọn **Contains**
4. OK → tất cả sản phẩm có "Chair" trong tên đều hiện (Office Chair, Executive Chair, Folding Chair...)

Các tùy chọn Wildcard:
- **Starts with**: bắt đầu bằng "Chair..."
- **Ends with**: kết thúc bằng "...Chair"
- **Contains**: chứa "Chair" ở bất kỳ vị trí nào
- **Exactly matches**: khớp chính xác

### H. Hiển thị Filter Cards trên Dashboard

Sau khi tạo filter trong worksheet, bạn có thể hiển thị nó dưới dạng control tương tác để người dùng tự chọn:

1. Click chuột phải vào filter pill trong Filters shelf
2. Chọn **Show Filter**
3. Filter hiện dưới dạng card trên view (dropdown, checkbox, slider...)
4. Thay đổi kiểu hiển thị: click chuột phải vào filter card → chọn kiểu (Single Value Dropdown, Multiple Values List, Slider...)

Trên dashboard, filter cards tự động xuất hiện khi bạn thêm worksheet có filter đang Show. Người dùng chỉ cần click chọn giá trị và chart cập nhật ngay lập tức.

### I. Apply to Worksheets — Áp dụng filter cho nhiều sheet

Mặc định, mỗi filter chỉ ảnh hưởng worksheet hiện tại. Để filter áp dụng cho nhiều sheet (cần thiết khi làm dashboard):

1. Click chuột phải vào filter pill → **Apply to Worksheets**
2. Chọn một trong các tùy chọn:
   - **Only This Worksheet** (mặc định): chỉ sheet hiện tại
   - **Selected Worksheets...**: chọn các sheet cụ thể
   - **All Using This Data Source**: tất cả sheets dùng cùng data source
   - **All Using Related Data Sources**: tất cả sheets dùng data source liên quan

Đây là bước quan trọng khi xây dựng dashboard — nếu quên thiết lập, người dùng chọn filter trên một chart nhưng chart khác không thay đổi theo.

## IV. Parameters — Tạo dashboard tương tác

### A. Parameters là gì?

**Parameters** (tham số) là giá trị động mà **người dùng nhập vào** — có thể là một con số, một chuỗi, hay một ngày. Khác với Filters lấy dữ liệu từ dataset, Parameters hoàn toàn do người dùng kiểm soát. Khi parameter thay đổi, tất cả Calculated Fields dùng parameter đó sẽ tự động cập nhật, kéo theo view thay đổi.

Nói cách khác: **Filters** trả lời câu hỏi "Hiện dữ liệu nào?", còn **Parameters** trả lời "Hiển thị theo cách nào?". Filters giới hạn dữ liệu, Parameters thay đổi logic phân tích.

### B. Cách tạo Parameter

Có 2 cách tạo parameter:

**Cách 1 — Từ Data Pane:**

1. Click chuột phải vào vùng trống trong **Data Pane**
2. Chọn **Create Parameter...**
3. Đặt tên, chọn kiểu dữ liệu, đặt giá trị mặc định
4. Cấu hình **Allowable values** (cho phép nhập gì):
   - **All**: nhập bất kỳ giá trị nào
   - **List**: chọn từ danh sách định sẵn
   - **Range**: chọn trong khoảng min–max
5. Click **OK**

**Cách 2 — Từ field có sẵn:**

1. Click chuột phải vào field trong Data Pane
2. Chọn **Create** → **Parameter...**
3. Tableau tự động điền danh sách giá trị từ field đó

### C. Kiểu dữ liệu của Parameter

Tableau hỗ trợ 6 kiểu dữ liệu cho parameter, mỗi kiểu phù hợp với mục đích khác:

| Kiểu | Mô tả | Ví dụ sử dụng |
|:-----|:------|:--------------|
| **Integer** | Số nguyên | Số lượng Top N hiển thị (5, 10, 20) |
| **Float** | Số thập phân | Ngưỡng tỷ lệ tăng trưởng (0.05, 0.10) |
| **String** | Chuỗi văn bản | Tên region, tên measure muốn xem |
| **Boolean** | True/False | Bật/tắt hiển thị reference line |
| **Date** | Ngày | Ngày bắt đầu / kết thúc kỳ phân tích |
| **Date & Time** | Ngày + Giờ | Thời điểm cụ thể (ít dùng trong dashboard) |

Chọn kiểu dữ liệu phù hợp giúp Tableau hiển thị control đúng loại — Integer/Float hiện slider, String hiện dropdown, Date hiện calendar picker.

### D. Dùng Parameter trong Calculated Field

Tạo parameter xong mới chỉ là bước đầu — bạn cần **dùng nó trong Calculated Field** để tạo logic động. Dưới đây là 3 ví dụ từ đơn giản đến phức tạp:

**Ví dụ 1: Thay đổi ngưỡng phân loại khách hàng**

Tạo parameter **"Sales Threshold"** (Integer, mặc định 5000, Allowable values: Range từ 1000 đến 50000, Step 1000):

```
// Calculated Field: Customer Tier (dynamic)
IF SUM([Sales]) > [Sales Threshold] THEN "VIP"
ELSE "Regular"
END
```

Khi người dùng kéo slider từ 5000 lên 10000, ít khách hàng hơn sẽ được xếp vào "VIP" — và chart cập nhật ngay lập tức. Đây là cách tạo phân tích what-if đơn giản mà hiệu quả.

**Ví dụ 2: Chọn measure hiển thị (Dynamic Measure Switching)**

Tạo parameter **"Select Measure"** (String, List: "Sales", "Profit", "Quantity"):

```
// Calculated Field: Dynamic Measure
CASE [Select Measure]
WHEN "Sales" THEN SUM([Sales])
WHEN "Profit" THEN SUM([Profit])
WHEN "Quantity" THEN SUM([Quantity])
END
```

Kéo field "Dynamic Measure" vào Rows hoặc Columns → người dùng chọn measure qua dropdown → biểu đồ tự chuyển giữa Sales, Profit, Quantity mà không cần tạo nhiều chart riêng biệt.

**Ví dụ 3: Top N sản phẩm**

Tạo parameter **"Top N"** (Integer, mặc định 10, Range: 1–50):

```
// Calculated Field: Product Rank
RANK(SUM([Sales]))
```

Dùng kết hợp: kéo Product Name vào Rows, Sales vào Columns, tạo Calculated Field filter:

```
// Calculated Field: Is Top N
[Product Rank] <= [Top N]
```

Kéo "Is Top N" vào Filters → chọn True. Khi người dùng thay đổi parameter Top N, danh sách sản phẩm hiển thị sẽ tự động cập nhật.

### E. Hiển thị Parameter Control

Để người dùng tương tác với parameter, bạn cần hiển thị control:

1. Click chuột phải vào parameter trong Data Pane
2. Chọn **Show Parameter Control**
3. Một hộp điều khiển xuất hiện trên view
4. Kiểu hiển thị tùy thuộc cấu hình:
   - **Allowable All/Range** → text box hoặc slider
   - **Allowable List** → dropdown hoặc radio buttons
5. Tùy chỉnh: click chuột phải vào control → chọn kiểu hiển thị mong muốn

### F. Parameter vs Filter — Khi nào dùng cái nào?

Câu hỏi thường gặp: "Parameter và Filter nhìn giống nhau, khi nào dùng cái nào?" Bảng dưới đây phân biệt rõ:

| Tiêu chí | Filter | Parameter |
|:----------|:-------|:----------|
| **Nguồn giá trị** | Lấy từ dataset (tự động cập nhật) | Người dùng định nghĩa (cố định) |
| **Phạm vi ảnh hưởng** | Thường chỉ 1 worksheet | Ảnh hưởng **tất cả** worksheets dùng nó |
| **Loại giá trị** | Chỉ chọn từ dữ liệu có sẵn | Bất kỳ giá trị nào (kể cả không có trong data) |
| **Dùng khi** | Lọc dữ liệu (hiện/ẩn) | Thay đổi logic phân tích (ngưỡng, measure, kỳ so sánh) |

**Kết hợp Filter + Parameter** là cách mạnh nhất — parameter tạo logic, filter thực hiện lọc. Ví dụ: parameter chọn ngưỡng VIP → calculated field phân loại → filter chỉ hiện VIP.

### G. Parameter Actions (Tableau 2020+)

Từ phiên bản 2020, Tableau hỗ trợ **Parameter Actions** — cho phép thay đổi parameter bằng cách **click trực tiếp vào visualization**, không cần nhập thủ công.

Ví dụ: click vào một Region trên biểu đồ → parameter được cập nhật thành tên region đó → các chart khác lọc theo region mới. Đây là cách tạo "drill-down" tương tác rất hiệu quả.

**Cách tạo Parameter Action:**

1. Menu **Worksheet** → **Actions...**
2. Click **Add Action** → chọn **Change Parameter**
3. Chọn **Source sheet** (nơi click) và **Target parameter** (parameter sẽ thay đổi)
4. Chọn **Source field** (giá trị nào sẽ truyền vào parameter — ví dụ: [Region])
5. Cấu hình **Clearing the selection** (khi bỏ chọn thì parameter lấy giá trị gì)
6. OK

## V. Kết hợp Filters và Parameters — Bài toán thực tế

### Dashboard phân tích doanh thu

Chúng ta sẽ áp dụng toàn bộ kiến thức đã học để xây dựng một hệ thống phân tích hoàn chỉnh. Dưới đây là các bước chi tiết:

**Bước 1: Tạo Parameters**

Tạo 2 parameters:
- **Region Selector** (String, List: "All", "East", "West", "Central", "South", mặc định "All")
- **Top N Products** (Integer, Range: 1–50, mặc định 10)

**Bước 2: Tạo Calculated Fields**

```
// 1. Filter động theo Region (dùng parameter)
// Nếu chọn "All" → hiện tất cả, nếu chọn region cụ thể → lọc
[Region Selector] = "All" OR [Region] = [Region Selector]
```

```
// 2. Profit Margin
ZN(SUM([Profit])) / ZN(SUM([Sales]))
```

```
// 3. Customer Classification (dùng aggregate)
IF SUM([Sales]) > 10000 THEN "VIP"
ELSEIF SUM([Sales]) > 5000 THEN "Regular"
ELSE "New"
END
```

**Bước 3: Thiết lập Filters**

- Kéo "Region Filter" (Calculated Field boolean) vào Filters → chọn **True**
- Kéo **Category** vào Filters → Show Filter → để người dùng tự chọn
- Tạo Top N filter: kéo Product Name vào Filters → tab **Top** → Top N by SUM(Sales) → ở ô "N", chọn parameter **Top N Products** thay vì nhập số cố định

**Bước 4: Hiển thị Parameter Controls**

- Click chuột phải vào "Region Selector" → Show Parameter Control
- Click chuột phải vào "Top N Products" → Show Parameter Control

**Bước 5: Tạo Worksheets**

- **Sheet 1 — KPI Cards**: Tổng Sales, Profit, Margin (dùng BAN — Big Ass Numbers)
- **Sheet 2 — Bar Chart**: Top N Products theo doanh thu
- **Sheet 3 — Line Chart**: Xu hướng doanh thu theo tháng

Tất cả sheets đều dùng chung parameters và filters, nên khi người dùng thay đổi Region hoặc Top N, toàn bộ cập nhật đồng bộ.

**Kết quả kỳ vọng:** Sau khi hoàn thành, dashboard nên có 3 sheets liên kết. Khi chọn Region = "East", cả 3 sheets chỉ hiện dữ liệu East. Khi kéo slider Top N từ 10 xuống 5, bar chart giảm còn 5 thanh. Khi chọn Region = "All", tất cả dữ liệu hiển thị lại.

### Thứ tự áp dụng Filters — Recap

Khi dashboard chạy, Tableau xử lý theo thứ tự:

```
Data Source Filters (nếu có)
    → Context Filters (nếu có)
    → Region Filter (Dimension Filter, dùng parameter)
    → Category Filter (Dimension Filter)
    → Top N Filter (Dimension Filter)
    → Measure Filters (nếu có)
    → Table Calculations (Running Total, Rank...)
```

> **💡 Tip — Tối ưu hiệu năng dashboard**
>
> - Dùng **Context Filter** cho filter loại bỏ phần lớn dữ liệu (như Year = 2024) — giúp các filter sau chạy nhanh hơn
> - Hạn chế dùng **Measure Filters** vì chúng chạy muộn nhất — thay bằng Calculated Field + Dimension Filter khi có thể
> - Tránh quá nhiều filters trên một worksheet — mỗi filter thêm thời gian xử lý

## VI. Tóm tắt buổi học

Trong buổi học này, chúng ta đã trang bị 3 công cụ cốt lõi:

**Calculated Fields:**
- Tạo chỉ số mới: Profit Margin, Customer Tier, Delivery Days
- Phân biệt row-level (từng dòng) và aggregate (nhóm dòng)
- LOD Expressions (FIXED, INCLUDE, EXCLUDE) — tính ở mức chi tiết bất kỳ
- Order of Operations — thứ tự 9 bước Tableau xử lý dữ liệu
- Quick Table Calculations — Running Total, Rank, Percent of Total

**Filters:**
- 6 loại filters từ Extract đến Table Calc, mỗi loại ở giai đoạn khác nhau
- Context Filter — "đẩy" filter lên trước FIXED LOD và Top N
- Dimension Filters (General, Wildcard, Condition, Top) cho categories
- Measure Filters cho giá trị số aggregate

**Parameters:**
- Giá trị động do người dùng nhập — không lấy từ dataset
- Kết hợp với Calculated Fields: ngưỡng động, measure switching, Top N
- Parameter Actions (2020+) — click vào chart để thay đổi parameter
- Một parameter ảnh hưởng tất cả worksheets dùng nó

## VII. Bài tập thực hành

### Bài tập: Sales Analysis Dashboard

**Dữ liệu:** Dataset Superstore (có sẵn trong Tableau Desktop)

**Yêu cầu:**

1. **Tạo Calculated Fields:**
   - **Profit Margin**: tỷ suất lợi nhuận (`ZN(SUM([Profit])) / ZN(SUM([Sales]))`)
   - **Customer Tier**: phân loại VIP / Regular / New dựa trên tổng Sales (dùng parameter cho ngưỡng)
   - **Delivery Days**: số ngày giao hàng (`DATEDIFF`)
   - **Customer Total Sales** (LOD): `{ FIXED [Customer Name] : SUM([Sales]) }` — tổng doanh thu mỗi khách bất kể view

2. **Tạo Parameters:**
   - **VIP Threshold** (Integer, mặc định 5000, Range 1000–50000)
   - **Top N** (Integer, mặc định 10, Range 1–50)

3. **Tạo Worksheets + Filters:**
   - **Bar Chart**: Top N sản phẩm theo doanh thu (dùng parameter Top N)
   - **Scatter Plot**: Profit Margin vs Customer Total Sales — xem khách hàng nào vừa mua nhiều vừa lãi cao
   - **Text Table**: Danh sách khách hàng + Customer Tier + Delivery Days trung bình
   - Thêm **Year filter** làm **Context Filter** để Top N đúng theo năm
   - Thêm **Category filter** hiển thị checkbox cho người dùng chọn

4. **Show Parameter Controls** cho VIP Threshold và Top N

**Hướng dẫn:**
- Tạo Calculated Fields trước, kiểm tra từng field bằng cách kéo vào view
- Nhớ format Profit Margin thành Percentage
- Kiểm tra Top N có đúng khi thay đổi parameter không
- Thử thay đổi VIP Threshold và quan sát Customer Tier thay đổi

## VIII. Tài liệu tham khảo

1. Tableau Official Documentation — Calculated Fields
   - https://help.tableau.com/current/pro/desktop/en-us/calculations_calculatedfields.htm

2. Tableau Official Documentation — Filters
   - https://help.tableau.com/current/pro/desktop/en-us/filtering.htm

3. Tableau Official Documentation — Parameters
   - https://help.tableau.com/current/pro/desktop/en-us/parameters.htm

4. Tableau Official Documentation — LOD Expressions
   - https://help.tableau.com/current/pro/desktop/en-us/calculations_calculatedfields_lod.htm

5. Tableau Official Documentation — Order of Operations
   - https://help.tableau.com/current/pro/desktop/en-us/order_of_operations.htm

6. Superstore Dataset — Sample Workbooks
   - Menu: **Help → Sample Workbooks → Superstore**
