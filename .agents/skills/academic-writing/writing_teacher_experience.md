# Writing Teacher — Nhật ký kinh nghiệm

> File này được agent tự động cập nhật sau mỗi lần viết bài.
> **QUAN TRỌNG:** Đọc file này ĐẦU TIÊN trước khi viết bài mới.

---

## Lỗi đã mắc & bài học

| # | Lỗi | Bối cảnh | Cách đã sửa | Bài học |
|:-:|:----|:---------|:------------|:-------|
| 1 | Đánh số subsection bị xáo trộn (A→B→C→**E**→D→F) | `tableau_preprocessing_functions.md` Section III | Đổi lại thứ tự: C→D(Type Conversion)→E(Ad-hoc)→F(Logical) | Luôn kiểm tra thứ tự chữ cái/số trước khi submit |
| 2 | Lỗi đánh máy: `táng` thay vì `tháng`, `trỪn` thay vì `trên` | Cả 2 bài Tableau | Sửa trực tiếp | Cần đọc lướt lại để phát hiện typo trước khi nộp bài |
| 3 | Hình ảnh placeholder chưa được điền — ✅ **ĐÃ SỬA** bằng cách trích xuất từ PDF nguồn | Cả 2 bài Tableau | Render PDF → PNG (pymupdf dpi=200), crop vùng hình cần → save vào `images/`, chèn `![caption](images/file.png)` + italics caption | Quy trình: (1) render tất cả trang PDF, (2) xem từng trang tìm hình phù hợp, (3) crop vùng cần, (4) chèn inline với caption |
| 4 | Section E của Preprocessing gộp 2 chủ đề không liên quan: Ad-hoc Calculation + xử lý NULL | `tableau_preprocessing_functions.md` | Tách thành 2 section độc lập | Mỗi section chỉ nên có 1 chủ đề rõ ràng |
| 5 | Typo trong sections mới viết: "kých thước" (kích), "mảt người" (mắt), tham chiếu "3 loại" cũ | `tableau_basic_charts.md` sections VI, VIII, X | Sửa trực tiếp qua review | Khi mở rộng bài → luôn chạy review pass toàn bộ file, đặc biệt sections mới + tham chiếu cũ |

## Mẫu viết hiệu quả

| # | Chủ đề | Cấu trúc | Tại sao hiệu quả |
|:-:|:-------|:---------|:-----------------|
| 1 | Discrete vs Continuous Date (Basic Charts) | Bảng so sánh + ví dụ "đường bị đứt" + hướng dẫn khắc phục | Bắt đúng pain point thực tế của người dùng Tableau mới |
| 2 | Pattern "Khi nào dùng" cho từng chart type | Câu hỏi phân tích → Chart → Lý do | Xuất phát từ câu hỏi, không phải từ data — đúng tư duy phân tích |
| 3 | Quick Reference Table cuối bài (Preprocessing) | "Vấn đề thực tế" → "Hàm xử lý" → "Ví dụ nhanh" | Sắp xếp theo góc nhìn người dùng (problem-first) thay vì tên hàm |
| 4 | Pattern chèn hình: `![alt](path)` + `*Caption italic*` | Hình → Caption italic → Giải thích trong text | Đúng chuẩn AIO2025: luôn cho xem hình trước, caption mô tả, text tham chiếu |

## Mẹo viết bài

1. **Hình ảnh Tableau là bắt buộc** — không thể để placeholder. Chụp màn hình Tableau Desktop ngay khi viết từng bước.
2. **Bảng tổng kết nên dùng "Vấn đề thực tế" làm cột đầu**, không dùng tên hàm — giúp người đọc tra cứu theo nhu cầu thay vì phải nhớ tên hàm.
3. **Kiểm tra thứ tự đánh số subsection** (A, B, C...) trước khi nộp — đặc biệt khi reorder section trong quá trình viết.
4. **Mỗi section chỉ 1 chủ đề** — không gộp 2 chủ đề khác nhau vào cùng một subsection dù chúng "có phần liên quan".
5. **Compute Using trong Quick Table Calculation**: đừng quên giải thích tại sao chọn "Cell" vs "Table" — đây thường là điểm gây nhầm lẫn nhất.
6. **Quy trình chèn hình từ PDF**: render PDF → view_file tìm trang phù hợp → crop/copy vào `images/` → chèn `![Hình N: mô tả](images/file.png)` + dòng italic `*Hình N: caption chi tiết*`. Luôn đặt hình ngay SAU đoạn văn mô tả bước liên quan.
