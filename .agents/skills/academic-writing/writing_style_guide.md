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
