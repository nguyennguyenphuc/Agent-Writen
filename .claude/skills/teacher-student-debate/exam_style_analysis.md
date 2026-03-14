# Phân tích phong cách ra đề và giải — Hướng dẫn tổng quát

> Đúc kết các nguyên tắc ra đề + giải chất lượng cao
> Dùng để bổ sung skill Teacher-Student Debate cho **bất kỳ chủ đề nào**

---

## 1. Cấu trúc bộ đề

Một bộ đề tốt nên có **8-12 câu**, độ khó tăng dần, pha trộn **nhiều loại câu hỏi**:

| STT | Loại câu hỏi | Mô tả | Kỹ năng kiểm tra |
|:---:|:-------------|:------|:-----------------|
| 1-2 | **Khái niệm + nhận diện** | Phân biệt/so sánh các khái niệm cơ bản, có thể kèm hình ảnh | Hiểu bản chất |
| 3-4 | **So sánh phương pháp** | So sánh 2+ phương pháp/kỹ thuật, có minh họa | Phân tích ưu nhược điểm |
| 5-6 | **Áp dụng công thức / Tính toán đơn giản** | Thay số vào công thức, tính kết quả | Vận dụng kiến thức |
| 7-8 | **Tính toán step-by-step / Suy luận** | Tính toán nhiều bước hoặc suy luận chuỗi logic | Hiểu cơ chế hoạt động |
| 9-10 | **Bài tổng hợp / End-to-end** | Kết hợp nhiều kiến thức, tính toán nặng | Tổng hợp toàn diện |
| 11-12 | **Lý thuyết ứng dụng / Thực hành** | Giải thích tại sao dùng kỹ thuật X, hoặc viết code | Tư duy thiết kế + lập trình |

> **Lưu ý:** Số câu và phân bổ loại câu hỏi linh hoạt tùy chủ đề. Quan trọng là đảm bảo **đa dạng** và **tăng dần độ khó**.

## 2. Phong cách ra đề

### 2a. Giới thiệu bối cảnh trước khi hỏi

Mỗi câu đều có **1-2 đoạn văn giới thiệu** bối cảnh/lý thuyết liên quan TRƯỚC khi đặt câu hỏi. Không hỏi trống trơn.

**Mẫu:**
> *"Trong lĩnh vực [X], phương pháp [Y] được sử dụng rộng rãi vì [lý do]. Tuy nhiên, [hạn chế]. Để khắc phục, kỹ thuật [Z] đã được đề xuất..."*
> → Rồi MỚI đặt câu hỏi.

### 2b. Có hình minh họa kèm theo

Hầu hết các câu nên có **hình vẽ / sơ đồ / bảng** kèm theo:
- Sơ đồ kiến trúc / luồng dữ liệu
- Bảng so sánh các phương pháp
- Hình minh họa input → xử lý → output
- ASCII diagram cho các pipeline/quy trình

### 2c. Đơn giản hóa bài toán cho tính toán bằng tay

Với bài tính toán, cần đơn giản hóa tự nhiên:
> *"Để đơn giản cho việc tính toán bằng tay, các tham số được cố định như sau: ..."*
> → Mô tả cụ thể hiệu quả của việc đơn giản hóa, không chỉ nêu giả định trừu tượng.

### 2d. Giả định đặc biệt dùng "(Lưu ý: ...)"

Khi có giả định đặc biệt hoặc quy ước khác chuẩn:
- Dùng `(Lưu ý: ...)` để ghi rõ
- Ví dụ: *(Lưu ý: không xét trường hợp X)*, *(Lưu ý: giả sử Y = Z)*

### 2e. Trắc nghiệm 4 đáp án

- Tất cả câu trắc nghiệm đều có **4 đáp án A/B/C/D**
- **Đáp án nhiễu phải hợp lý** — là kết quả khi tính sai 1 bước hoặc hiểu nhầm 1 khái niệm
- Đáp án có thể là: text, số, ma trận, hình ảnh, đoạn code... tùy loại câu hỏi

## 3. Phong cách giải (Solution)

### 3a. Giải thích từng đáp án (loại trừ)

Với câu lý thuyết: giải thích **TẠI SAO** mỗi đáp án đúng/sai:
> - *Choice A sai: vì...*
> - *Choice B đúng: vì...*
> - *Choice C sai: vì...*
> - *Choice D sai: vì...*

### 3b. Bài tính toán: step-by-step đầy đủ

- Viết ra **toàn bộ kết quả trung gian** (không bỏ bước)
- Mỗi phép tính ghi rõ: input → phép toán → output
- Với bài có nhiều bước: đánh số thứ tự rõ ràng

### 3c. Công thức → thay số → kết quả

Với câu dùng công thức, luôn theo 3 dòng:
> 1. *Công thức: [viết công thức]*
> 2. *Thay số: [thay giá trị cụ thể]*
> 3. *Kết quả: [đáp án]*

### 3d. Đề xuất verify bằng code

Khi có bài tính toán, khuyến khích: *"Có thể kiểm tra lại bằng code"* — kèm gợi ý cách verify.

## 4. Checklist chất lượng đề

### Cho Teacher (ra đề):

1. **Mỗi câu phải có BỐI CẢNH** — 1-2 đoạn giới thiệu lý thuyết trước khi hỏi
2. **Hình minh họa kèm theo** — sơ đồ, bảng, hình vẽ, không chỉ text
3. **Đa dạng loại câu hỏi** — pha trộn: khái niệm, so sánh, công thức, tính toán, code
4. **Độ khó tăng dần** — từ nhận diện → vận dụng → tổng hợp → sáng tạo
5. **Giả định ghi "(Lưu ý: ...)"** — rõ ràng, tự nhiên, không trừu tượng
6. **Đáp án nhiễu = kết quả khi tính sai** — không random, phải có logic

### Cho Student (giải bài + review solution):

1. **Solution phải giải thích TẤT CẢ đáp án** — không chỉ đáp án đúng
2. **Kết quả trung gian đầy đủ** — không bỏ bước
3. **Công thức → thay số → kết quả** — 3 dòng rõ ràng
4. **Đề xuất verify bằng code** khi có bài tính toán

---

## 5. Văn phong & Kỹ thuật dẫn dắt

> Trích xuất từ đề POS-Tagging (AIO2025) — 10 câu, quiz-with-solution style.

### 5a. Mở đầu bằng hành động (không mở bằng định nghĩa)

Dùng **động từ mệnh lệnh** hoặc **lời mời quan sát** để mở đầu câu hỏi thay vì định nghĩa khô khan.

| ❌ Sai | ✅ Đúng (từ POS-Tagging quiz) |
|:-------|:------------------------------|
| "POS Tagging là bài toán gán nhãn..." | **"Quan sát hình minh hoạ kiến trúc mô hình bên dưới:"** (Câu 1) |
| "Self-attention là cơ chế..." | **"Xét self-attention 1 head với $d_k=2$..."** (Câu 3) |
| "CrossEntropyLoss là hàm..."  | **"Xét bài toán POS Tagging với nhãn PRP..."** (Câu 4) |

**Mẫu mở đầu hay:**
- *"Quan sát hình minh hoạ [X] bên dưới..."*
- *"Xét [bài toán / cơ chế / mô hình] với [thiết lập cụ thể]..."*
- *"Cho hình minh hoạ [phép toán / quá trình]..."*
- *"Vẫn sử dụng notebook của câu [N]..."*

### 5b. Luồng dẫn dắt: Bối cảnh → Thiết lập → Hình → Câu hỏi

Mỗi câu hỏi đi theo luồng 4 bước rõ ràng:

```
1. BỐI CẢNH:  1-2 câu giới thiệu ngắn (tại sao bài toán này quan trọng?)
2. THIẾT LẬP:  Cho dữ kiện cụ thể (số, ma trận, code...)
3. HÌNH:       Sơ đồ / hình minh họa (nếu có)
4. CÂU HỎI:   1 câu rõ ràng, bắt đầu bằng "Phát biểu nào...", "Hãy chọn...", "Hỏi..."
```

**Ví dụ Câu 4 (POS-Tagging):**
1. Bối cảnh: *"Xét bài toán POS Tagging với nhãn PRP có index là 0"*
2. Thiết lập: *labels = [0,1,2,0,0], loss từng token, 2 cách config*
3. (Không hình)
4. Câu hỏi: *"Hỏi giá trị loss trung bình xấp xỉ bao nhiêu?"*

### 5c. So sánh song song (A) vs (B)

Khi muốn test khả năng phân tích, đặt **2 phương án** cạnh nhau:

**Mẫu từ quiz:**
> *"Xét hai cách tiếp cận sau:*
> - *(A) One-word classification: mô hình nhận một token đơn lẻ...*
> - *(B) Sentence + token-wise MLP: mô hình nhận cả câu...*
> 
> *Phát biểu nào sau đây là đúng nhất về hạn chế của (A) và (B)?"*

**Kỹ thuật:**
- Mô tả A và B bằng **cùng cấu trúc** (input → process → output)
- Đáp án trắc nghiệm so sánh cả A lẫn B cùng lúc
- Đáp án nhiễu: hiểu đúng A nhưng sai B, hoặc ngược lại

### 5d. Câu hỏi code: cho code → hỏi về code

Không mô tả thuật toán bằng lời rồi hỏi — **cho thẳng code** rồi hỏi:

**Mẫu từ quiz (Câu 5, 6, 7, 8):**
> *"Cho hình minh hoạ phép flatten:"*
> [hình]
> [code block]
> *"Hãy chọn đoạn code đúng nhất để tính loss..."*

**Kỹ thuật:**
- Code phải **chạy được** (không pseudo-code)
- Đáp án trắc nghiệm là 4 đoạn code khác nhau, mỗi đoạn sai 1 chỗ
- Hoặc: cho code đúng → hỏi kết quả chạy

### 5e. Chuỗi câu liên tiếp (tiếp tục notebook)

Nhiều câu nối tiếp nhau cùng 1 bài toán, tạo cảm giác **làm lab thực tế**:

**Mẫu:**
> Câu 6: *"Cho hình minh hoạ quá trình Data Processing trong [link notebook]"*
> Câu 7: *"Tiếp tục với notebook của Câu 6..."*
> Câu 8: *"Quan sát hàm tính accuracy trong [notebook]..."*
> Câu 9: *"Vẫn sử dụng notebook của câu 8..."*

**Kỹ thuật:**
- Câu đầu giới thiệu notebook + cho link
- Các câu sau chỉ nói "Tiếp tục..." hoặc "Vẫn sử dụng..."
- Mỗi câu test 1 khía cạnh khác nhau của cùng bài toán

### 5f. Câu cuối: bài toán ứng dụng thực tế

Câu cuối cùng thường là **bài toán thực tế** (không tính toán), test tư duy thiết kế:

**Mẫu Câu 10 (POS-Tagging):**
> *"Cho bài toán xây dựng một hệ thống tóm tắt ý kiến khách hàng cho một sàn thương mại điện tử..."*
> [hình biểu đồ ứng dụng]
> *"Để giảm thiểu nhiễu... bạn nên giữ lại token thuộc nhóm POS Tags nào?"*

**Kỹ thuật:**
- Mô tả scenario thực tế cụ thể (ngành, mục tiêu)
- Có hình minh họa output mong muốn
- Đáp án test khả năng áp dụng kiến thức vào thực tế

### 5g. In đậm có chiến lược

Sử dụng **bold** để highlight đúng chỗ, giúp sinh viên scan nhanh:

| Vị trí | Cái gì được bold |
|:-------|:-----------------|
| Đầu câu | Số câu: **Câu 1**, **Câu 2**... |
| Trong bối cảnh | Khái niệm chính: **Depthwise Conv**, **POS Tagging** |
| Trong đáp án | Từ khóa phân biệt: **"chính xác nhất"**, **"đúng nhất"** |
| Trong solution | Label đúng/sai: **"Choice A sai:"**, **"Choice B đúng:"** |
| Thiết lập đặc biệt | Giá trị quan trọng: **mask đúng**, **mask sai** |

---

## 6. Hành văn AIO2025 (từ YOLO tutorials)

> Trích xuất từ YoloV1 (Exercise), YoloV5 (Tutorial), YoloV10 (Tutorial) — AIO2025.
> Đây là văn phong chuẩn cho tài liệu giảng dạy + đề thi của khóa học.

### 6a. Giọng văn thân mật, cùng khám phá

Dùng **"chúng ta"** và **ngôi thứ nhất số nhiều**, tạo cảm giác cùng học:

| ❌ Giọng học thuật | ✅ Giọng AIO2025 |
|:-------------------|:----------------|
| "Bài toán Classification được định nghĩa là..." | *"Đầu tiên, chúng ta sẽ xây dựng một mô hình Classification đơn giản để làm nền tảng..."* |
| "Phương pháp Object Detection có thể được mô tả..." | *"Trong buổi hôm nay, chúng ta sẽ cùng nhau xây dựng model YOLO cho bài toán Object Detection"* |
| "YOLOv10 được đề xuất bởi..." | *"Thời gian vừa qua, Ao Wang và các cộng sự đã đề xuất mô hình YOLOv10..."* |

**Các cụm thường dùng:**
- *"Chúng ta sẽ đi qua 4 phần chính:..."*
- *"Trong project này, chúng ta sẽ tìm hiểu và thực hành..."*
- *"Để bắt đầu, chúng ta cần..."*
- *"Tiếp theo, chúng ta sẽ..."*

### 6b. Thuật ngữ: bold + giải nghĩa tiếng Việt trong ngoặc

Khi dùng thuật ngữ tiếng Anh lần đầu, **bold** và kèm **nghĩa tiếng Việt trong ngoặc**:

**Mẫu từ YOLO tutorials:**
- ***Object Detection** (Tạm dịch: Phát hiện đối tượng)* là một bài toán...
- *...không chỉ phân loại mà còn xác định vị trí bằng **bounding box***
- *...sử dụng thuật toán có tên gọi là **YOLOv5***

**Quy tắc:**
- Lần đầu xuất hiện: **bold** + giải nghĩa
- Các lần sau: dùng thuật ngữ gốc (không cần giải thích lại)
- Thuật ngữ phổ biến (model, loss, dataset): không cần dịch

### 6c. Cấu trúc "Giải thích → Code → Giải thích"

Mỗi đoạn code được kẹp giữa 2 lớp giải thích:

```
1. GIẢI THÍCH TRƯỚC: "Để quản lý dữ liệu, chúng ta sẽ định nghĩa một class ImageDataset..."
2. CODE: [code block với line numbers]
3. GIẢI THÍCH SAU:  "Trong class ImageDataset, chúng ta thực hiện các bước sau:
   • Khởi tạo: Xác định thư mục...
   • Lọc dữ liệu: Loại bỏ...
   • Đếm object: ..."
```

**Đặc biệt:** Giải thích sau thường dùng **bullet list** với từ khóa **bold** đầu mỗi mục.

### 6d. Hình trước, giải thích sau

Luôn **cho xem hình trước**, rồi mới giải thích nội dung hình:

**Mẫu YoloV1:**
> [Hình 1: Hình ảnh minh họa cho 3 bài toán Classification, Localization và Object Detection]
>
> *"Trong hình ảnh minh họa ở Figure 1, chúng ta có thể thấy sự khác biệt giữa ba bài toán chính: **Classification** chỉ đơn giản là phân loại..., **Localization** không chỉ phân loại mà còn..., và cuối cùng là **Object Detection** kết hợp cả hai..."*

**Kỹ thuật:**
- Hình có caption: *"Hình X: [mô tả ngắn]"*
- Sau hình: giải thích bằng cách **tham chiếu từng phần** trong hình
- Dùng bold cho tên khái niệm tương ứng với phần được chỉ trong hình

### 6e. Dẫn dắt từ đơn giản → phức tạp

Cấu trúc toàn bài đi từ bài toán đơn giản đến phức tạp, mỗi bước xây trên bước trước:

**Mẫu YoloV1 (4 bước):**
1. Classification (1 object) — *"để làm nền tảng"*
2. Classification + Localization — *"sau đó thêm vào"*
3. Multi-object Classification + Localization
4. YOLOv1 — *"và cuối cùng là"*

**Cụm nối giữa các bước:**
- *"Đầu tiên... để làm nền tảng cho các bài toán phức tạp hơn"*
- *"Thay vì đi thẳng vào..., ta sẽ xây dựng từ... sau đó thêm vào..."*
- *"Cuối cùng là..."*

### 6f. Câu mệnh lệnh cho yêu cầu thực hành

Khi yêu cầu sinh viên làm gì, dùng **câu mệnh lệnh rõ ràng**:

**Mẫu:**
- *"Hãy điền đoạn code forward sau vào vị trí yêu cầu và chạy cell code"* (Quiz Câu 7)
- *"Các bạn sẽ đọc, tìm hiểu cách sử dụng mã nguồn YOLOv5 và áp dụng vào bộ dữ liệu human"* (YoloV5)
- *"Hãy thực hiện theo các bước hướng dẫn sau"*

### 6g. Câu nối và chuyển tiếp

Dùng các cụm nối để liên kết giữa các đoạn:

| Vị trí | Cụm nối |
|:-------|:--------|
| Bắt đầu phần mới | *"Tiếp theo..."*, *"Trong phần tiếp theo..."* |
| Giải thích lý do | *"Để thuận tiện trong..."*, *"Để đảm bảo tính nhất quán..."* |
| Tham chiếu hình | *"Trong hình minh họa ở Figure X, chúng ta có thể thấy..."* |
| Tham chiếu trước đó | *"Vẫn sử dụng... của câu X"*, *"Tiếp tục với..."* |
| Kết luận | *"Cuối cùng là..."*, *"Thông qua đó..."* |
