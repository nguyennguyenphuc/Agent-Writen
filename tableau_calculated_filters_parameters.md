# Phân tích nâng cao trong Tableau — Calculated Fields, Filters, Parameters

> **Dataset sử dụng trong ví dụ:** Tableau Superstore — bộ dữ liệu mẫu có sẵn trong Tableau Desktop, tại menu **Help → Sample Workbooks → Superstore**. Dataset gồm ~10.000 dòng đơn hàng bán lẻ với các trường về sản phẩm, khách hàng, doanh thu và lợi nhuận.

## I. Giới thiệu — Từ "nhìn dữ liệu" đến "hiểu dữ liệu"

Buổi học trước, chúng ta đã nắm được cách tạo các biểu đồ cơ bản và dùng Preprocessing Functions để làm sạch dữ liệu. Đó là những kỹ năng nền tảng — cho phép bạn trình bày thông tin một cách trực quan.

Nhưng một nhà phân tích dữ liệu giỏi không chỉ "nhìn" được dữ liệu. Họ còn phải "hiểu" được dữ liệu thông qua việc đặt câu hỏi, lọc ra những gì cần thiết, và tạo ra những chỉ số mới phục vụ mục đích cụ thể.

Trong buổi học này, chúng ta sẽ học 3 công cụ mạnh mẽ nhất của Tableau để làm điều đó:

- **Calculated Fields** — tạo chỉ số mới từ dữ liệu hiện có
- **Filters** — lọc dữ liệu để tập trung vào phần quan trọng
- **Parameters** — tạo dashboard tương tác cho phép người dùng tự chọn cách nhìn dữ liệu

Hãy bắt đầu với Calculated Fields — công cụ mà hầu như mọi dashboard chuyên nghiệp đều sử dụng.

## II. Calculated Fields nâng cao — Tạo chỉ số mới

### Tổng quan về Calculated Fields

Calculated Fields là cách tạo cột dữ liệu mới bằng cách viết biểu thức trực tiếp trong Tableau. Ở buổi trước, chúng ta đã dùng Calculated Fields cho các tác vụ preprocessing: chuyển đổi kiểu dữ liệu, làm sạch chuỗi, xử lý ngày tháng.

Nhưng sức mạnh thực sự của Calculated Fields nằm ở khả năng tạo ra những **chỉ số phân tích** (analytics metrics) hoàn toàn mới — những thứ không tồn tại trong dữ liệu gốc nhưng cực kỳ có giá trị cho việc ra quyết định kinh doanh.

### Các loại Calculated Fields

Tableau phân biệt 2 cấp độ tính toán:

| Loại | Mô tả | Ví dụ |
|:-----|:------|:-------|
| **Row-level** | Tính toán trên từng dòng dữ liệu | `[Sales] * 0.1` — tính thuế 10% cho mỗi đơn hàng |
| **Aggregate** | Tính toán trên nhóm dòng | `SUM([Sales]) / SUM([Quantity])` — doanh thu trung bình mỗi sản phẩm |

Điểm khác biệt rất quan trọng: row-level tính toán cho từng dòng trước khi aggregate, còn aggregate tính toán trên tổng. Viết sai loại sẽ cho kết quả sai.

### Aggregations thường dùng

Khi làm việc với Calculated Fields ở cấp aggregate, bạn sẽ dùng các hàm sau:

| Hàm | Ý nghĩa | Ví dụ |
|:-----|:---------|:-------|
| `SUM(tổng)` | Cộng tất cả giá trị | `SUM([Sales])` — tổng doanh thu |
| `AVG(trung bình)` | Trung bình cộng | `AVG([Profit])` — lợi nhuận trung bình |
| `MIN/NHỎ NHẤT` | Giá trị nhỏ nhất | `MIN([Order Date])` — ngày đặt hàng đầu tiên |
| `MAX/LỚN NHẤT` | Giá trị lớn nhất | `MAX([Order Date])` — ngày đặt hàng gần nhất |
| `COUNT(đếm)` | Đếm số dòng | `COUNT([Order ID])` — tổng số đơn hàng |
| `COUNTD(đếm duy nhất)` | Đếm giá trị duy nhất | `COUNTD([Customer ID])` — số khách hàng |

Một điều cần nhớ: trong Calculated Fields, khi bạn dùng cả aggregate (như SUM) và non-aggregate (như [Discount]) cùng nhau, Tableau sẽ báo lỗi "Cannot mix aggregate and non-aggregate arguments". Giải pháp là đảm bảo tất cả các field trong công thức đều ở cùng cấp độ.

### Ví dụ thực tế — Tạo chỉ số kinh doanh

Chúng ta sẽ tạo một số chỉ số quan trọng từ dataset Superstore:

**1. Tỷ suất lợi nhuận (Profit Margin)**

```
// Tỷ suất lợi nhuận = Lợi nhuận / Doanh thu
// Dùng ZN để xử lý trường hợp Sales = 0
ZN(SUM([Profit])) / ZN(SUM([Sales]))
```

**2. Doanh thu trung bình mỗi đơn hàng**

```
// Tổng doanh thu / Số đơn hàng
SUM([Sales]) / COUNT([Order ID])
```

**3. Số ngày giao hàng**

```
// Số ngày từ lúc đặt đến lúc giao
DATEDIFF('day', [Order Date], [Ship Date])
```

**4. Phân loại khách hàng theo doanh thu**

```
// Phân loại dựa trên tổng doanh thu
IF SUM([Sales]) > 10000 THEN "VIP"
ELSEIF SUM([Sales]) > 5000 THEN "Regular"
ELSE "New"
END
```

> **💡 Tip — Kiểm tra nhanh Calculated Field**
>
> Sau khi tạo Calculated Field, kéo nó vào view và quan sát. Nếu thấy "ABC" (string) hay "#" (number) ở pill thì biểu thức đúng. Nếu thấy dấu màu cam bên cạnh tên field trong công thức tức là Tableau đang ở mức row-level.

### LOD Expressions — Khi cần tính toán ở cấp khác

**LOD (Level of Detail) Expressions** là một tính năng nâng cao nhưng cực kỳ mạnh mẽ. Nó cho phép bạn tính toán ở một mức độ chi tiết (level) khác với mức đang hiển thị trên biểu đồ.

Ví dụ: bạn muốn tính tổng doanh thu của mỗi khách hàng, rồi so sánh với doanh thu trung bình chung. Nếu dùng cách thông thường, bạn phải tạo view với Customer Name ở chi tiết rồi mới tính.

Với LOD, bạn có thể làm trong một công thức duy nhất:

```
{ FIXED [Customer Name] : SUM([Sales]) }
```

Công thức này nói: "Tính tổng doanh thu cho mỗi khách hàng, bất kể view đang hiển thị gì."

Tableau có 3 loại LOD:

| Loại | Ý nghĩa | Ví dụ |
|:-----|:---------|:-------|
| **FIXED** | Tính theo dimensions chỉ định, bỏ qua mọi thứ khác | `{ FIXED [Region] : SUM([Sales]) }` |
| **INCLUDE** | Thêm dimensions vào mức hiện tại | `{ INCLUDE [Category] : AVG([Profit]) }` |
| **EXCLUDE** | Loại trừ dimensions khỏi mức hiện tại | `{ EXCLUDE [Region] : SUM([Sales]) }` |

**Ví dụ so sánh:**

Giả sử bạn có biểu đồ doanh thu theo Category. Bạn muốn thêm một line hiển thị tổng doanh thu toàn quốc để so sánh.

- Dùng `SUM([Sales])` — sẽ cho kết quả theo từng Category (Furniture: X, Technology: Y)
- Dùng `{ FIXED : SUM([Sales]) }` — sẽ cho một con số duy nhất là tổng toàn quốc, hiển thị như một đường thẳng cho tất cả các Category

### Thứ tự thực hiện trong Tableau

Khi làm việc với Calculated Fields, bạn cần hiểu thứ tự Tableau xử lý dữ liệu. Thứ tự này quyết định kết quả cuối cùng:

```
1. Data Source → 2. Filters → 3. LOD → 4. Table Calculations → 5. Visual
```

Điều này có nghĩa:
- Filters áp dụng **trước** khi Calculated Fields tính toán
- LOD expressions tính toán **sau** Filters nhưng **trước** các aggregation thông thường
- Table Calculations (như Running Total, Percent of Total) tính **sau cùng** trên dữ liệu đã được hiển thị

### Quick Table Calculations — Tính toán nhanh trên dữ liệu đã hiển thị

Quick Table Calculations là tính năng cho phép bạn thực hiện các phép tính phổ biến (như Running Total, Rank, Percent of Total) mà không cần viết công thức thủ công. Điểm khác biệt với Calculated Fields thông thường là: Quick Table Calculations tính toán **trên dữ liệu đã được hiển thị** trong view, không phải trên toàn bộ dataset.

#### Các loại Quick Table Calculations:

| Loại | Mô tả | Ví dụ sử dụng |
|:-----|:------|:---------------|
| **Running Total** | Cộng dồn từ đầu đến cuối | Doanh thu tích lũy theo tháng |
| **Percent of Total** | Tính phần trăm trên tổng | Tỷ trọng doanh thu mỗi quý |
| **Percent Difference** | Chênh lệch phần trăm so với kỳ trước | Tăng trưởng doanh thu tháng này so với tháng trước |
| **Rank** | Xếp hạng | Top 10 sản phẩm bán chạy |
| **Moving Average** | Trung bình động | Xu hướng doanh thu trung bình 3 tháng gần nhất |
| **YTD Total** | Tổng từ đầu năm đến hiện tại | Doanh thu từ đầu năm |

#### Cách tạo Quick Table Calculations:

1. Kéo field (ví dụ `Sales`) vào view
2. Click chuột phải vào pill
3. Chọn **Quick Table Calculation**
4. Chọn loại calculation mong muốn

#### Cấu hình Compute Using:

Sau khi chọn loại calculation, bạn cần chỉ định **Compute Using** — tức là theo hướng nào để tính toán:

- **Table (across)**: Tính theo chiều ngang bảng
- **Table (down)**: Tính theo chiều dọc bảng
- **Table (across then down)**: Tính theo chiều ngang rồi xuống
- **Table (down then across)**: Tính theo chiều dọc rồi sang phải
- **Specific Dimension**: Chỉ định dimension cụ thể để tính

**Ví dụ Running Total:**

Giả sử bạn có biểu đồ doanh thu theo từng tháng (Order Date theo tháng). Để xem doanh thu cộng dồn:

1. Click chuột phải vào `SUM(Sales)` trên Columns/Rows
2. Chọn **Quick Table Calculation** → **Running Total**
3. Kết quả: tháng 1 = X, tháng 2 = X + Y, tháng 3 = X + Y + Z...

**Ví dụ Percent of Total:**

Để xem tỷ trọng doanh thu mỗi quý trong tổng năm:

1. Kéo `Sales` vào view, đặt theo Quarter
2. Click chuột phải → **Quick Table Calculation** → **Percent of Total**
3. Đảm bảo **Compute Using** đúng (thường là Table across down)

> **💡 Tip — Kiểm tra kết quả Quick Table Calculation**
>
> Quick Table Calculations có thể cho kết quả không như mong đợi nếu Compute Using không đúng. Luôn kiểm tra bằng cách hover vào các điểm trên chart để xem giá trị chi tiết.

#### So sánh Quick Table Calculations vs Calculated Fields:

| Tiêu chí | Quick Table Calculations | Calculated Fields |
|:----------|:------------------------|:------------------|
| Cách tạo | Click chuột phải, chọn menu | Viết công thức thủ công |
| Tính toán | Trên dữ liệu đã hiển thị | Trên toàn bộ dataset |
| Độ linh hoạt | Ít linh hoạt, chỉ các loại predefined | Linh hoạt hoàn toàn |
| Hiệu năng | Chậm hơn với dữ liệu lớn | Nhanh hơn |

## III. Filters — Lọc dữ liệu để tập trung

### Tại sao cần Filters?

Khi làm việc với dataset lớn, bạn thường không muốn hiển thị tất cả dữ liệu. Ví dụ: chỉ xem doanh thu của năm 2024, chỉ xem khách hàng VIP, hoặc chỉ xem các sản phẩm có lợi nhuận dương. Filters giúp bạn làm điều đó.

### Các loại Filters trong Tableau

Tableau có 4 loại Filters, mỗi loại hoạt động ở một giai đoạn khác nhau trong quá trình xử lý dữ liệu:

| Loại Filter | Thứ tự | Mô tả | Hiệu năng |
|:------------|:-------|:------|:----------|
| **Extract** | 1 (sớm nhất) | Lọc khi import dữ liệu vào Tableau | Nhanh nhất |
| **Data Source** | 2 | Lọc trước khi vào worksheet | Nhanh |
| **Context** | 3 | Lọc tạo "bối cảnh" cho các filter khác | Trung bình |
| **Dimension** | 4 | Lọc theo categories (Region, Category...) | Trung bình |
| **Measure** | 5 | Lọc theo giá trị số (Sales > 1000) | Chậm nhất |
| **Table Calculation** | 6 (cuối cùng) | Lọc sau khi tính toán xong | Chậm nhất |

### Cách tạo Filter

**Cách 1 — Kéo thả:**

1. Trong Data Pane, kéo field bạn muốn lọc vào khung **Filters**
2. Tableau hiện hộp thoại cho phép chọn giá trị cần hiển thị
3. Chọn giá trị → OK

**Cách 2 — Click chuột phải:**

1. Click chuột phải vào field trong Data Pane
2. Chọn **Show Filter**
3. Filter hiện ở khung bên phải, có thể tùy chỉnh giao diện

### Filter theo Dimension

Dimension Filters lọc theo các giá trị phân loại — Region, Category, Customer Name...

Ví dụ: lọc chỉ hiển thị dữ liệu của khu vực **East** và **West**:

```
// Tạo Calculated Field làm filter
[Region] = "East" OR [Region] = "West"
```

Kéo field này vào Filters, chọn **True** → chỉ East và West hiển thị.

### Filter theo Measure

Measure Filters lọc dựa trên giá trị số — doanh thu, lợi nhuận, số lượng...

Ví dụ: chỉ hiển thị những sản phẩm có doanh thu trên 1 triệu:

1. Kéo **Sales** vào Filters
2. Chọn kiểu **Range of Values** hoặc **At Least**
3. Nhập giá trị 1,000,000
4. OK

### Context Filter — Filter đặc biệt

Context Filter là một khái niệm quan trọng nhưng hay bị hiểu sai. Khi bạn đánh dấu một filter là "Add to Context", nó sẽ:

- **Chạy trước** các filter thông thường
- Trở thành "nền" mà các filter khác hoạt động trên đó

Ví dụ: bạn muốn xem top 10 khách hàng theo doanh thu **trong năm 2024**.

- Nếu không dùng Context: Tableau sẽ tính top 10 trên toàn bộ dữ liệu, rồi mới lọc năm 2024 — kết quả có thể không đúng
- Nếu dùng Context: lọc Year = 2024 **trước**, rồi tính top 10 **sau** — kết quả đúng

**Cách tạo Context Filter:**

1. Click chuột phải vào filter trong khung Filters
2. Chọn **Add to Context**
3. Filter sẽ chuyển sang màu xám đậm

### Wildcard Filter — Lọc theo mẫu

Khi cần lọc theo một phần của chuỗi, dùng Wildcard Filter.

Ví dụ: lọc tất cả khách hàng có tên bắt đầu bằng "John":

1. Kéo **Customer Name** vào Filters
2. Chọn **Wildcard**
3. Nhập "John" và chọn **Starts with**
4. OK

### Filter Cards trên Dashboard

Khi đã tạo filter trong worksheet, bạn có thể hiển thị nó trên dashboard để người dùng tự chọn:

1. Trong worksheet, click chuột phải vào filter trong khung
2. Chọn **Show Filter**
3. Filter hiện dưới dạng card trên view
4. Trên dashboard, filter sẽ tự động xuất hiện như một điều khiển tương tác

## IV. Parameters — Dashboard tương tác

### Parameters là gì?

Parameters là một giá trị động mà người dùng có thể thay đổi để ảnh hưởng đến cách hiển thị dữ liệu. Khác với Filters lấy dữ liệu từ dataset, Parameters là giá trị do **người dùng nhập vào** — có thể là một con số, một chuỗi, hay một ngày.

Điểm mạnh của Parameters là nó cho phép bạn tạo một dashboard mà người dùng có thể tùy chỉnh cách nhìn dữ liệu mà không cần biết viết code.

### Cách tạo Parameter

1. Click chuột phải vào một field trong Data Pane
2. Chọn **Create** → **Parameter...**
3. Đặt tên, chọn kiểu dữ liệu, đặt giá trị mặc định
4. Click **OK**

Bạn cũng có thể tạo parameter trống rồi dùng trong Calculated Fields:

1. Trong khung Data Pane, click mũi tên bên cạnh biểu tượng **Filters**
2. Chọn **Create Parameter**
3. Cấu hình tương tự

### Kiểu dữ liệu của Parameter

| Kiểu | Mô tả | Ví dụ sử dụng |
|:-----|:------|:---------------|
| **Integer** | Số nguyên | Ngưỡng doanh thu để phân loại khách hàng |
| **Float** | Số thập phân | Tỷ lệ phần trăm tăng trưởng |
| **String** | Chuỗi văn bản | Tên region muốn xem |
| **Boolean** | True/False | Hiện/ẩn dữ liệu |
| **Date** | Ngày | Ngày bắt đầu/kết thúc |
| **Date & Time** | Ngày + Giờ | Thời điểm cụ thể |

### Dùng Parameter trong Calculated Field

Sau khi tạo parameter, bạn cần dùng nó trong Calculated Field để tạo logic động.

**Ví dụ 1: Thay đổi ngưỡng phân loại khách hàng**

Tạo parameter tên **"Sales Threshold"** (kiểu Integer, mặc định 5000):

```
// Calculated Field dùng parameter
IF SUM([Sales]) > [Sales Threshold] THEN "VIP"
ELSE "Regular"
END
```

Khi người dùng thay đổi giá trị trong parameter, kết quả phân loại sẽ tự động cập nhật.

**Ví dụ 2: Chọn kỳ so sánh**

Tạo parameter tên **"Compare Period"** (kiểu String, các giá trị: "Year", "Quarter", "Month"):

```
// Calculated Field tạo dimension động
CASE [Compare Period]
WHEN "Year" THEN STR(YEAR([Order Date]))
WHEN "Quarter" THEN "Q" + STR(DATEPART('quarter', [Order Date]))
WHEN "Month" THEN DATENAME('month', [Order Date])
END
```

**Ví dụ 3: Top N sản phẩm**

Tạo parameter tên **"Top N"** (kiểu Integer, mặc định 10):

Dùng trong Calculated Field:

```
// Tạo rank theo Sales
RANK(SUM([Sales]))
```

Sau đó tạo filter: `[Rank] <= [Top N]`

### Hiển thị Parameter Control

Để người dùng có thể tương tác với parameter:

1. Click chuột phải vào parameter trong Data Pane
2. Chọn **Show Parameter Control**
3. Một hộp điều khiển xuất hiện trên view
4. Người dùng có thể thay đổi giá trị bằng cách nhập, chọn từ list, hoặc dùng slider (tùy cấu hình)

### Parameter kết hợp với Filter

Parameters mạnh hơn Filters ở chỗ: một parameter có thể điều khiển nhiều worksheets cùng lúc, trong khi filter thường chỉ ảnh hưởng một worksheet.

Cách kết hợp:

1. Tạo parameter với danh sách giá trị cho trước (Allowable values → List)
2. Dùng parameter trong Calculated Field như filter
3. Hiển thị parameter control trên dashboard
4. Người dùng chọn giá trị → tất cả charts liên quan đều cập nhật

### Parameter Actions (Tableau 2020+)

Từ phiên bản 2020, Tableau hỗ trợ **Parameter Actions** — cho phép thay đổi parameter bằng cách click trực tiếp vào visualization.

Ví dụ: click vào một Region trên map → parameter được cập nhật thành tên region đó → các chart khác lọc theo region mới.

Cách tạo:

1. Menu **Worksheet** → **Actions...**
2. Chọn **Add Action** → **Change Parameter**
3. Chọn source sheet và target parameter
4. Cấu hình cách lấy giá trị từ click

### Dashboard Actions — Tạo tính tương tác

Dashboard Actions là cách tạo tương tác giữa các thành phần trên dashboard. Ngoài Filter Actions và Parameter Actions đã đề cập, còn có các loại khác:

#### 1. Highlight Actions

Highlight Actions không lọc dữ liệu — nó chỉ **nhấn mạnh** các giá trị được chọn bằng cách làm mờ các giá trị khác.

**Ví dụ sử dụng:**
- Khi người dùng click vào một Category trên biểu đồ, các thanh tương ứng trên tất cả các chart khác sẽ nổi bật hơn

**Cách tạo:**

1. Menu **Dashboard** → **Actions...**
2. Chọn **Add Action** → **Highlight**
3. Chọn source sheet (nơi người dùng click)
4. Chọn target sheets (nơi hiệu ứng highlight áp dụng)
5. Chọn field để highlight (thường là field dùng làm dimension chính)

#### 2. URL Actions

URL Actions cho phép mở một trang web hoặc file khi người dùng click vào một mark trên biểu đồ.

**Ví dụ sử dụng:**
- Click vào tên khách hàng để mở email
- Click vào sản phẩm để mở trang sản phẩm
- Click vào vùng trên map để mở website vùng đó

**Cách tạo:**

1. Menu **Dashboard** → **Actions...**
2. Chọn **Add Action** → **URL**
3. Nhập URL cố định hoặc dùng field để tạo URL động:
   - Ví dụ: `"https://example.com/customer/" + [Customer ID]`

> **💡 Tip — URL Actions với Image**
>
> Bạn có thể dùng URL Actions để hiển thị ảnh động khi hover qua một mark. Điều này hữu ích khi muốn hiển thị thêm thông tin mà không chiếm không gian trên dashboard.

#### 3. Go to Sheet Actions

Go to Sheet Actions cho phép chuyển sang một worksheet khác khi người dùng click.

**Ví dụ sử dụng:**
- Click vào một vùng trên map để chuyển đến worksheet chi tiết của vùng đó
- Click vào một danh mục để xem chi tiết phân tích sâu hơn

**Cách tạo:**

1. Menu **Dashboard** → **Actions...**
2. Chọn **Add Action** → **Go to Sheet**
3. Chọn sheet đích

#### 4. Set Actions (Tableau 2018+)

Set Actions cho phép người dùng thêm/bớt giá trị vào một Set thông qua tương tác trên dashboard.

**Ví dụ sử dụng:**
- Người dùng click chọn nhiều sản phẩm để so sánh
- Người dùng chọn các tháng để xem xu hướng

**Cách tạo:**

1. Tạo một Set trước (click chuột phải vào field → Create → Set)
2. Menu **Dashboard** → **Actions...**
3. Chọn **Add Action** → **Change Set Values**
4. Chọn source và target sheets

### Dashboard Layout — Thiết kế giao diện

Khi xây dựng dashboard, bạn cần quyết định cách sắp xếp các thành phần. Tableau cung cấp 2 phương pháp chính:

#### Tiled (ghép) vs Floating (nổi)

| Đặc điểm | Tiled | Floating |
|:----------|:------|:---------|
| **Cách hoạt động** | Các đối tượng được xếp tự động theo lưới | Đặt ở vị trí tuyệt đối |
| **Kích thước** | Tự động thay đổi theo kích thước dashboard | Cố định |
| **Tương thích** | Tự động resize tốt | Cần điều chỉnh thủ công |
| **Độ phức tạp** | Đơn giản | Linh hoạt hơn |
| **Sử dụng khi** | Dashboard đơn giản | Dashboard phức tạp, overlay |

**Khi nào dùng Tiled:**
- Dashboard đơn giản, ít thành phần
- Muốn tự động responsive với kích thước màn hình
- Không cần kiểm soát chính xác vị trí

**Khi nào dùng Floating:**
- Dashboard phức tạp với nhiều layers
- Cần overlay (ví dụ: tooltip tùy chỉnh)
- Cần đặt elements chính xác

#### Containers

Containers giúp nhóm các đối tượng lại với nhau và kiểm soát layout tốt hơn.

| Loại Container | Mô tả |
|:---------------|:-------|
| **Horizontal** | Chứa các đối tượng xếp ngang |
| **Vertical** | Chứa các đối tượng xếp dọc |

**Cách sử dụng:**
1. Trên dashboard, vào menu **Objects**
2. Chọn **Horizontal** hoặc **Vertical**
3. Kéo các sheets vào container

#### Show/Hide Buttons

Buttons cho phép ẩn/hiện các thành phần trên dashboard — hữu ích để tiết kiệm không gian.

**Cách tạo:**

1. Trên dashboard, vào **Objects** → **Show/Hide Button**
2. Đặt button vào vị trí mong muốn
3. Click chuột phải vào button → **Edit Button**
4. Chọn worksheet cần ẩn/hiện

### Reference Lines — Đường tham chiếu

Reference Lines là các đường thẳng hoặc vùng được thêm vào biểu đồ để so sánh — giúp người xem nhanh chóng đánh giá xem giá trị đang cao hơn hay thấp hơn mức nào đó.

#### Các loại Reference Lines:

| Loại | Mô tả |
|:-----|:------|
| **Reference Line** | Một đường thẳng tại giá trị cụ thể |
| **Reference Band** | Một vùng giữa hai giá trị |
| **Distribution Band** | Vùng dựa trên phân phối (std dev, percentiles) |

#### Cách tạo:

1. Kéo field vào biểu đồ
2. Trong Analytics pane, kéo **Reference Line** vào vị trí mong muốn
3. Chọn scope (per table, per pane, per cell)
4. Cấu hình giá trị và format

**Ví dụ Reference Line:**
- Thêm đường doanh thu trung bình vào biểu đồ doanh thu theo sản phẩm → nhanh chóng thấy sản phẩm nào trên/dưới trung bình

**Ví dụ Reference Band:**
- Thêm vùng mục tiêu (target range) vào biểu đồ → thấy các tháng đạt mục tiêu hay không

> **⚠️ Lưu ý — Reference Lines và Filters**
>
> Reference Lines có thể bị ảnh hưởng bởi filters. Nếu muốn reference line cố định (không đổi khi filter), chọn **"Per table"** scope thay vì **"Per pane"** hoặc **"Per cell"**.

## V. Kết hợp Filters và Parameters trong thực tế

### Bài toán: Dashboard phân tích doanh thu

Chúng ta sẽ áp dụng tất cả kiến thức đã học để xây dựng một dashboard có tính tương tác cao.

**Yêu cầu:**
- Cho phép người dùng chọn khu vực (Region)
- Cho phép người dùng chọn số lượng sản phẩm top hiển thị (Top N)
- Cho phép lọc theo danh mục (Category)
- Tự động tính các chỉ số: tổng doanh thu, lợi nhuận, tỷ suất

**Bước 1: Tạo Parameters**

- **Region Selector** (String): Danh sách các region + "All"
- **Top N Products** (Integer): Giá trị mặc định 10

**Bước 2: Tạo Calculated Fields**

```
// Filter động theo Region
[Region] = [Region Selector]
// hoặc nếu chọn "All"
[Region Selector] = "All" OR [Region] = [Region Selector]

// Tính rank theo Sales
RANK(SUM([Sales]))

// Top N filter
[Rank] <= [Top N Products]
```

**Bước 3: Tạo chỉ số tổng hợp**

```
// Tổng doanh thu đã lọc
SUM([Sales])

// Lợi nhuận
SUM([Profit])

// Tỷ suất lợi nhuận
ZN(SUM([Profit])) / ZN(SUM([Sales]))
```

**Bước 4: Thiết lập Filters**

- Kéo Category vào filter (Dimension Filter)
- Kéo calculated field Rank vào filter, chọn True

**Bước 5: Tạo Dashboard**

- Thêm parameter controls để người dùng chọn Region và Top N
- Sắp xếp layout với KPIs ở trên, charts ở dưới

### Thứ tự áp dụng Filters trên Dashboard

Khi có nhiều filters, thứ tự ảnh hưởng đến hiệu năng và kết quả:

```
1. Data Source Filters (chạy trên database)
2. Context Filters
3. Dimension Filters
4. Measure Filters
5. Top N / Bottom N Filters
6. Table Calculation Filters (chạy cuối cùng)
```

> **💡 Tip — Tối ưu hiệu năng**
>
> - Dùng Context Filter cho những filter loại bỏ phần lớn dữ liệu (như Year = 2024)
> - Measure Filters chạy chậm nhất — cân nhắc dùng Calculated Field + Dimension Filter thay thế
> - Tránh dùng too many filters trên một worksheet — mỗi filter thêm thời gian xử lý

## VI. Tóm tắt buổi học

Trong buổi học này, chúng ta đã học 3 công cụ quan trọng:

**Calculated Fields:**
- Tạo chỉ số mới từ dữ liệu hiện có
- Phân biệt row-level và aggregate calculations
- LOD Expressions cho phép tính toán ở mức độ chi tiết khác nhau
- Hiểu thứ tự xử lý để viết công thức đúng

**Filters:**
- 4 loại filters hoạt động ở các giai đoạn khác nhau
- Context Filter tạo nền cho các filter khác
- Dimension Filters cho categories, Measure Filters cho số
- Wildcard Filters cho lọc theo mẫu text

**Parameters:**
- Giá trị động do người dùng nhập
- Kết hợp với Calculated Fields để tạo dashboard tương tác
- Parameter Actions cho phép click để thay đổi giá trị
- Một parameter có thể điều khiển nhiều worksheets

Kết hợp 3 công cụ này, bạn có thể xây dựng những dashboard chuyên nghiệp, cho phép người dùng tự khám phá dữ liệu mà không cần biết viết code.

## VII. Bài tập thực hành

### Bài tập 1: Tạo Sales Dashboard

**Dữ liệu:** Dataset Superstore

**Yêu cầu:**

1. **Tạo các Calculated Fields:**
   - Profit Margin (tỷ suất lợi nhuận)
   - Customer Classification (VIP/Regular/New dựa trên tổng Sales)
   - Ship Time (số ngày giao hàng)

2. **Tạo Parameters:**
   - Region Parameter: cho phép chọn một hoặc tất cả các region
   - Year Parameter: cho phép chọn năm phân tích

3. **Tạo Filters:**
   - Lọc theo Category (Dimension Filter)
   - Lọc chỉ hiển thị sản phẩm có Profit > 0 (Measure Filter)

4. **Tạo Dashboard:**
   - Hiển thị tổng Sales, Profit, và Margin đã lọc
   - Biểu đồ doanh thu theo Region (map hoặc bar chart)
   - Top 10 sản phẩm theo doanh thu (dùng parameter để thay đổi số lượng)
   - Biểu đồ xu hướng doanh thu theo thời gian (line chart)
   - Kết hợp tất cả vào một dashboard với parameter controls

**Hướng dẫn:**

- Dùng Region Parameter trong Calculated Field để tạo filter động
- Dùng Year Parameter kết hợp với Date Functions để lọc dữ liệu theo năm
- Top N: tạo RANK field rồi filter theo parameter

---

## VIII. Tài liệu tham khảo

1. Tableau Official Documentation — Calculated Fields
   - https://help.tableau.com/current/pro/desktop/en-us/calculations_calculatedfields.htm

2. Tableau Official Documentation — Filters
   - https://help.tableau.com/current/pro/desktop/en-us/filtering.htm

3. Tableau Official Documentation — Parameters
   - https://help.tableau.com/current/pro/desktop/en-us/parameters.htm

4. Tableau LOD Expressions Explained
   - https://help.tableau.com/current/pro/desktop/en-us/lod_calcs.htm

5. Superstore Dataset — Sample Workbooks
   - Menu: **Help → Sample Workbooks → Superstore**
