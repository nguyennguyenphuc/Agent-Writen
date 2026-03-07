# Phong cách viết AIO2025 — Hướng dẫn chi tiết

> Tổng hợp từ tài liệu mẫu (smolagents tutorial) + phong cách ra đề AIO2025.
> Áp dụng cho **viết tutorial, tài liệu giảng dạy** — không phải đề thi.

---

## 1. Cấu trúc bài viết chuẩn

Một bài viết hoàn chỉnh theo chuẩn AIO2025 có cấu trúc sau:

| Phần | Nội dung | Ví dụ từ tài liệu mẫu |
|:-----|:---------|:----------------------|
| **Header** | Logo + tên tổ chức + loại tài liệu + tiêu đề + tác giả | `AI VIET NAM – AI COURSE 2025 / Tutorial: smolagents` |
| **I. Giới thiệu** | Bối cảnh + Vấn đề + Input/Output + Hình tổng quan | Giới thiệu AI Agent, bài toán tổng hợp tin tức |
| **Mục lục** | Tự động từ `\tableofcontents` | I → II → III → IV → V → Phụ lục |
| **II. Kiến thức nền** | Tổng quan + Kiến trúc + Quy trình + Các loại/Phương pháp | Thư viện smolagents, ReAct framework |
| **III. Thực hành** | Cài đặt + Từng bước xây dựng + Kết quả | Cài đặt agent, định nghĩa tools, chạy agent |
| **IV. Câu hỏi** | Trắc nghiệm kiểm tra kiến thức (tùy chọn) | 4 đáp án A/B/C/D |
| **V. Tài liệu tham khảo** | BibTeX references | Papers, GitHub repos, courses |
| **Phụ lục** | Datasets, Hints, Solutions, Rubric | Links tải code, rubric đánh giá |

## 2. Hành văn & Giọng điệu

### 2a. Giọng thân mật, cùng khám phá

Dùng **"chúng ta"** và **ngôi thứ nhất số nhiều**, tạo cảm giác cùng học:

| ❌ Giọng hàn lâm | ✅ Giọng AIO2025 |
|:-----------------|:----------------|
| "AI Agent được định nghĩa là..." | *"AI Agents (Tạm dịch: Các tác tử AI) là các phần mềm ứng dụng công nghệ AI tự động nhận yêu cầu từ người dùng..."* |
| "Thư viện X hỗ trợ..." | *"smolagents là một thư viện đơn giản nhưng mạnh mẽ được phát triển bởi Hugging Face, giúp xây dựng các AI agents chỉ với vài dòng code"* |
| "Phương pháp này có hiệu quả..." | *"Cách tiếp cận này được chứng minh là hiệu quả hơn đáng kể so với các phương pháp truyền thống"* |

**Cụm thường dùng:**
- *"Trong bối cảnh [X], [vấn đề]. Do đó, bài viết này hướng đến..."*
- *"Chúng ta sẽ lựa chọn [X] làm công cụ minh họa để..."*
- *"Chỉ với một số cài đặt đơn giản, chúng ta hoàn toàn có thể..."*
- *"Tiếp theo, chúng ta sẽ đi sâu vào..."*

### 2b. Thuật ngữ: bold + giải nghĩa tiếng Việt

Khi dùng thuật ngữ tiếng Anh lần đầu, **bold** và kèm **nghĩa tiếng Việt**:

**Mẫu từ tài liệu:**
- ***AI Agents** (Tạm dịch: Các tác tử AI)* là các phần mềm...
- *...kết hợp giữa **suy luận** (reasoning) và **hành động** (acting)*
- *Với phương pháp "**code-first**" hoặc "**tool-calling**"...*

**Quy tắc:**
- Lần đầu xuất hiện: **bold** + giải nghĩa trong ngoặc
- Các lần sau: dùng thuật ngữ gốc (không cần giải thích lại)
- Thuật ngữ phổ biến (model, loss, dataset, API): không cần dịch

### 2c. Câu nối và chuyển tiếp

| Vị trí | Cụm nối |
|:-------|:--------|
| Bắt đầu phần mới | *"Tiếp theo..."*, *"Trong phần này..."*, *"Cụ thể..."* |
| Giải thích lý do | *"Do đó..."*, *"Lý do là vì..."*, *"Nhờ khả năng..."* |
| Liệt kê | *"Cụ thể, thư viện có các đặc điểm nổi bật sau:"* |
| Ví dụ | *"Dưới đây là ví dụ minh họa cách..."* |
| Kết luận | *"Như vậy..."*, *"Thông qua đó..."* |

## 3. Patterns viết nội dung

### 3a. Pattern Giải thích → Code → Giải thích

Mỗi đoạn code được kẹp giữa **2 lớp giải thích**:

```
1. GIẢI THÍCH TRƯỚC (1-2 đoạn):
   "Chúng ta bắt đầu bằng việc [mục đích]. Sau đó, [giải thích sơ bộ]:"

2. CODE BLOCK:
   [code với line numbers]

3. GIẢI THÍCH SAU (bullet list hoặc bảng):
   "Trong ví dụ trên, [giải thích chi tiết]:"
   • Component 1: [giải thích]
   • Component 2: [giải thích]
   • Component 3: [giải thích]
```

**Ví dụ từ tài liệu mẫu:**

> *"Chúng ta bắt đầu bằng việc cài đặt các thư viện cần thiết:"*
> [code block: `!pip install "smolagents[transformers]"`]
> *"Sau đó, import các module cần thiết và thiết lập seed để đảm bảo tính tái lập kết quả:"*
> [code block: import + set_seed]

### 3b. Pattern Hình → Caption → Giải thích

Luôn **cho xem hình trước**, rồi mới giải thích:

```
1. HÌNH:    [hình/sơ đồ]
2. CAPTION: "Hình X: [mô tả ngắn]"
3. GIẢI THÍCH: "Trong hình minh họa ở Hình X, chúng ta có thể thấy..."
```

### 3c. Pattern Bảng tóm tắt

Dùng bảng để tóm tắt thông tin có cấu trúc:

**Mẫu:**
| Thành phần | Mô tả |
|:-----------|:------|
| **Model** | Là "bộ não" của agent, chịu trách nhiệm suy luận |
| **Tools** | Mở rộng khả năng bằng cách cung cấp chức năng |
| **Logs** | Ghi lại toàn bộ quá trình hoạt động |

**Kỹ thuật:**
- Bold tên thành phần ở cột 1
- Mô tả ngắn gọn, tự nhiên ở cột 2
- Dùng `Table X:` cho caption bảng chính thức

### 3d. Pattern Ví dụ step-by-step

Khi minh hoạ quy trình nhiều bước:

```
Bước 1: [Tên bước]
  - Mô tả ngắn gọn
  - [Hình/Code minh họa]
  - Giải thích kết quả

Bước 2: [Tên bước]
  ...

Kết quả: [Tóm tắt kết quả cuối cùng]
```

**Ví dụ từ tài liệu mẫu:** 3 vòng lặp của agent (get_coordinates → get_weather → final_answer), mỗi vòng có sơ đồ kiến trúc riêng.

### 3e. Pattern Bullet list đặc điểm

Khi liệt kê đặc điểm/ưu điểm:

```
Cụ thể, [X] có các đặc điểm nổi bật sau:
• **Tính đơn giản tối đa**: [giải thích]
• **Cách tiếp cận code-first**: [giải thích]
• **Hỗ trợ đa dạng**: [giải thích]
```

**Kỹ thuật:**
- Bold tên đặc điểm đầu mỗi bullet
- Giải thích tiếp sau dấu hai chấm
- Mỗi bullet 1-2 câu, không quá dài

## 4. Hộp thông tin

### Khi nào dùng loại nào:

| Hộp | Biểu tượng | Dùng khi |
|:----|:-----------|:---------|
| **Khái niệm** | ¶ | Định nghĩa hoặc giải thích khái niệm quan trọng |
| **Tip / Mẹo** | 💡 | Mẹo, hướng dẫn, quy tắc thực hành |
| **Lưu ý** | ⚠️ | Nhấn mạnh thông tin quan trọng, cảnh báo |
| **Yêu cầu** | ✅ | Yêu cầu sinh viên thực hiện task cụ thể |
| **Chú thích** | 📝 | Giải thích thuật ngữ, ghi chú bên lề |

**Nguyên tắc:**
- Không dùng quá nhiều hộp liên tiếp — tối đa 1-2 hộp/section
- Hộp Lưu ý: dành cho thông tin **thực sự quan trọng**, không lạm dụng
- Hộp Khái niệm: dùng cho khái niệm **lần đầu** xuất hiện

## 5. In đậm có chiến lược

| Vị trí | Cái gì được bold |
|:-------|:-----------------|
| Đầu phần | Tên section/subsection |
| Thuật ngữ mới | Lần đầu xuất hiện: **bold** + giải nghĩa |
| Đặc điểm | Đầu mỗi bullet: **Tên đặc điểm**: giải thích |
| Bảng tóm tắt | Tên cột 1 trong bảng |
| Nhấn mạnh | Từ khóa quan trọng trong đoạn văn |

## 6. Dẫn dắt từ đơn giản → phức tạp

Cấu trúc toàn bài đi từ bài toán đơn giản đến phức tạp:

**Mẫu từ tài liệu:**
1. **Giới thiệu** — *"AI Agents là gì? Tại sao cần?"*
2. **Tổng quan thư viện** — *"smolagents có gì nổi bật?"*
3. **Kiến trúc chi tiết** — *"Bên trong agent có gì?"*
4. **Quy trình hoạt động** — *"Agent hoạt động thế nào?"*
5. **Ví dụ step-by-step** — *"3 vòng lặp cụ thể"*
6. **Xây dựng ứng dụng** — *"Tự tay build agent"*

**Cụm nối giữa các mức:**
- *"Tiếp theo, chúng ta sẽ đi sâu vào..."*
- *"Để hiểu rõ hơn, chúng ta xem xét..."*
- *"Sau khi nắm được [X], chúng ta sẽ..."*

## 7. Checklist chất lượng bài viết

### Checklist tổng thể:

- [ ] Có giới thiệu bối cảnh + vấn đề ở đầu bài?
- [ ] Mục lục rõ ràng, đầy đủ?
- [ ] Giọng văn nhất quán (thân mật, "chúng ta")?
- [ ] Thuật ngữ mới: bold + giải nghĩa lần đầu?
- [ ] Có đủ hình/sơ đồ minh họa?
- [ ] Code chạy được, có giải thích trước + sau?
- [ ] Dẫn dắt từ đơn giản → phức tạp?
- [ ] Câu nối chuyển tiếp giữa các section?
- [ ] Có tài liệu tham khảo?
- [ ] Có phụ lục (datasets, hints, solutions)?

### Checklist mỗi section:

- [ ] Mở đầu bằng hành động, không bằng định nghĩa?
- [ ] Có ít nhất 1 hình/bảng/code block?
- [ ] Bullet list rõ ràng, bold đầu mỗi item?
- [ ] Hộp thông tin đúng loại, đúng chỗ?
- [ ] Không quá dài (≤ 2-3 trang/section)?

---

## 8. Mật độ văn bản — Tiêu chuẩn "đủ dày"

### 8a. Tiêu chuẩn đoạn văn

Đây là điểm **quan trọng nhất** mà bài viết sơ sài thường thiếu. Từ tài liệu mẫu AIO2025:

| ❌ Sơ sài | ✅ Đúng chuẩn AIO2025 |
|:---------|:----------------------|
| Đoạn 1-2 câu | Đoạn **4-6 câu liên tục**, giải thích đầy đủ bối cảnh + lý do + ý nghĩa |
| Bullet 1 dòng: tên hàm + chức năng | Bullet **2-3 dòng**: **bold tên**: giải thích cặn kẽ, ví dụ, tại sao quan trọng |
| Bảng hàm không có ví dụ | Bảng → ví dụ code → giải thích output → lưu ý edge case |
| "Hàm TRIM() xóa khoảng trắng" | "**TRIM()** xóa khoảng trắng ở **đầu và cuối** chuỗi — một lỗi cực kỳ phổ biến khi import dữ liệu từ CSV hoặc nhập tay. Nếu không loại bỏ khoảng trắng này, hai chuỗi `'Hà Nội'` và `' Hà Nội'` sẽ bị coi là hai giá trị khác nhau khi filter, dẫn đến kết quả sai." |

### 8b. Tiêu chuẩn độ dài mỗi phần

Dựa trên tài liệu mẫu (28 trang / 5 section lớn):

| Phần | Độ dài tối thiểu | Nội dung bắt buộc |
|:-----|:-----------------:|:-----------------|
| **Giới thiệu section** | 2-3 đoạn văn | Bối cảnh → Vấn đề → Giải pháp (bao giờ cũng có hình tổng quan) |
| **Subsection lý thuyết** | 1 đoạn giới thiệu + bảng/bullet chi tiết | Không bao giờ đặt bảng/bullet trực tiếp mà không có đoạn văn dẫn dắt |
| **Subsection ví dụ** | 1 đoạn setup + code + 1 đoạn giải thích output | Code block không bao giờ đứng trơ trọi |
| **Bullet item** | 2-3 dòng | **Bold tên**: 1-2 câu giải thích đủ nghĩa |
| **Hàm/function** | Cú pháp + Use case + Ví dụ + Lưu ý | Không chỉ liệt kê "Hàm X làm Y" |

### 8c. Pattern dẫn dắt trước subsection

Tài liệu mẫu **không bao giờ** đặt bảng hay bullet ngay sau tiêu đề section. Luôn có 1-2 đoạn văn giới thiệu:

```
## II.1. Tên subsection

[Đoạn 1: Giới thiệu tổng quan — subsection này nói về gì, tại sao cần,
bối cảnh thực tế. 3-4 câu.]

[Đoạn 2 (tùy chọn): Thu hẹp vào nội dung cụ thể sẽ trình bày. 2-3 câu.]

[Bảng / Bullet / Code block — bây giờ mới xuất hiện]
```

**Ví dụ từ tài liệu mẫu (trang 4):**
> *"smolagents là một thư viện đơn giản nhưng mạnh mẽ được phát triển bởi Hugging Face, giúp xây dựng các AI agents chỉ với vài dòng code. Mục tiêu chính của thư viện là đơn giản hóa quá trình phát triển ứng dụng AI thông minh, giảm thiểu sự phức tạp và các lớp trừu tượng không cần thiết vốn có ở nhiều framework hiện nay. Cụ thể, thư viện có các đặc điểm nổi bật sau:"*

→ Sau đó mới xuất hiện bullet list. **Không bao giờ** bắt đầu bằng bullet ngay.

### 8d. Tiêu chuẩn bullet item AIO2025

Mỗi bullet phải có **cấu trúc đầy đủ**:

```
• **Tên tính năng/khái niệm:** Giải thích rõ ràng tính năng đó làm gì.
  Nêu tại sao điều này quan trọng hoặc trường hợp sử dụng điển hình.
  Có thể thêm ví dụ ngắn hoặc so sánh nếu cần.
```

**Ví dụ từ tài liệu mẫu (trang 4):**
> *"• **Tính đơn giản tối đa:** Logic cho các agents trong smolagents chỉ khoảng 1000 dòng code, giúp code dễ đọc, dễ hiểu và dễ debug, giảm thiểu các lớp trừu tượng không cần thiết."*

---

## 9. Lỗi viết sơ sài — Cần tránh

### 9a. Danh sách lỗi thường gặp

| # | ❌ Lỗi | ✅ Cách sửa |
|:-:|:------|:-----------|
| 1 | Bảng hàm → xong, không có ví dụ thực tế | Sau bảng LUÔN có ví dụ code với output cụ thể |
| 2 | Ví dụ code quá ngắn (1-2 dòng) | Ví dụ đủ context: setup + action + output + giải thích |
| 3 | Bullet 1 dòng: "TRIM() — xóa khoảng trắng" | Bullet 2-3 dòng với use case và lý do quan trọng |
| 4 | Section bắt đầu ngay bằng bảng/bullet | LUÔN có 1-2 đoạn văn dẫn dắt trước |
| 5 | Giải thích hàm nhưng không nói khi nào dùng | Mỗi hàm: cú pháp + **khi nào dùng** + ví dụ |
| 6 | Code không có comment giải thích | Thêm comment `//`, hoặc giải thích từng phần sau code |
| 7 | Bảng không có câu giới thiệu | "Bảng X tổng hợp..." rồi mới đặt bảng |
| 8 | Subsection quá ngắn (< 1 trang) | Mỗi subsection ít nhất 1 trang đầy đủ |

### 9b. Self-check trước khi submit

Trước khi nộp bài, đọc lại và tự hỏi:

- **"Nếu tôi là sinh viên lần đầu học chủ đề này, tôi có hiểu không?"** — Nếu không, giải thích thêm.
- **"Đoạn này có bị copy-paste kiểu reference table không?"** — Nếu có, biến thành văn xuôi.
- **"Có bước nào bị bỏ qua vì tôi cho là 'sinh viên tự hiểu'?"** — Đừng assume, giải thích hết.
- **"Ví dụ có đủ context để chạy không?"** — Phải có data, setup, expected output.

