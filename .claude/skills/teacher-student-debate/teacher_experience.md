# Teacher — Nhật ký kinh nghiệm

> File này được agent tự động cập nhật sau mỗi lần ra đề.  
> **QUAN TRỌNG:** Đọc file này ĐẦU TIÊN trước khi ra đề mới.

---

## Lỗi đã mắc & bài học

| # | Lỗi | Bối cảnh | Cách đã sửa | Bài học |
|:-:|:----|:---------|:------------|:--------|
| 1 | Thứ tự DW/PW viết ngược trong bối cảnh | Câu 9 - Depthwise/Pointwise Conv | Thêm lưu ý thứ tự có thể thay đổi tùy mục đích | Luôn ghi rõ thứ tự các phép toán trong đề, đặc biệt khi paper mô tả khác với bài tập |
| 2 | Hỏi giá trị tại vị trí (1,1) nhưng không ghi 0-indexed hay 1-indexed | Câu 9 - PDF version (5×5, stride=2) | Cần thêm ghi chú "(sử dụng 0-indexed)" | Khi hỏi giá trị tại vị trí cụ thể, LUÔN ghi rõ index convention |
| 3 | Solution chỉ giải thích đáp án đúng, không giải thích các đáp án sai | Quiz_1 - Object Detection YOLO | Cần thêm giải thích loại trừ cho A, B, C | Solution câu lý thuyết PHẢI giải thích TẤT CẢ đáp án (đúng + sai) |
| 4 | Tiêu chí "vi phạm định nghĩa chuẩn" quá trừu tượng | Quiz_1 - Câu 1 matching task | Đề xuất đổi thành "output không thể hiện đặc trưng riêng" | Khi đặt tiêu chí đánh giá trong đề, viết CỤ THỂ — tránh từ trừu tượng như "chuẩn", "chính thức" |
| 5 | GT = 16 nhưng tính Recall sai (1/16 ≠ 0.08) → AP sai → không có đáp án đúng | Quiz_3_5 - Câu 2 AP calculation | Cần sửa GT = 12 hoặc tính lại toàn bộ | **LUÔN verify bằng Python** khi tính AP/mAP — sai 1 số ảnh hưởng toàn bộ chain |
| 6 | Câu hỏi phụ thuộc notebook ngoài — không self-contained | Quiz_3_5 - Câu 1 NMS | Cần thêm predictions data inline | Đề thi PHẢI self-contained — có đủ data để giải mà không cần tài nguyên ngoài |

## Mẫu đề hiệu quả

| # | Chủ đề | Cấu trúc đề | Tại sao hiệu quả |
|:-:|:-------|:------------|:-----------------|
| 1 | DW + PW Conv | 3 phần: (a) tính DW, (b) tính PW, (c) so sánh params | Chia nhỏ bài toán, mỗi phần xây trên phần trước. Câu (c) trắc nghiệm giúp kiểm tra nhanh |
| 2 | DW + PW Conv (PDF) | 1 câu hỏi: giá trị tại 1 vị trí | Quá ít — sinh viên có thể đoán mò. So sánh: bản 3 sub-questions hiệu quả hơn rõ rệt |
| 3 | Object Detection YOLO (Quiz_1) | Matching: nối hình output → tên bài toán, tìm hình sai | Ý tưởng hay, sơ đồ đẹp. Nhưng hình minh họa phải thể hiện **đặc trưng riêng** — tránh hình ambiguous |
| 4 | Object Detection YOLO (Quiz_2) | Bối cảnh chi tiết + hỏi 1 điểm yếu kỹ thuật + distracter logic | **MẪU TỐT**: bối cảnh đầy đủ, solution giải thích 4/4 đáp án, distracters test hiểu sâu. Điểm 22.5/25 |
| 5 | Object Detection YOLO (Quiz_4) | Cho S,B,C + sơ đồ kiến trúc → hỏi output shape | Đơn giản nhưng hiệu quả, test kiến thức cốt lõi. Sơ đồ đẹp. Điểm 23.5/25 |
| 6 | Object Detection YOLO (Quiz_8) | YOLOv10 Dual Label Assignment, tính soft label | **MẪU DISTRACTER TỐT NHẤT**: mỗi đáp án sai là giá trị trung gian thật (m_1, t_5, t_3). Điểm 20.5/25 |
| 7 | Object Detection YOLO (Quiz1-9 Câu 8) | DW+PW Conv: tính output tại vị trí cụ thể | Diagram copy nhầm X(0) cho X(1) — LUÔN cross-check diagram vs text. Answer C (4) đúng theo text |
| 8 | Object Detection YOLO (Quiz_6) | Lý thuyết: tại sao √w, √h trong box loss + minh chứng số | **MẪU SOLUTION TỐT NHẤT**: giải thích 4/4, controlled variable (Δ=10 cả 2), so sánh có/không √. Điểm 24/25 |
| 9 | Object Detection YOLO (Quiz_7) | λ tuning: tăng λ_noobj, giảm λ_coord → hiệu ứng | **MẪU DISTRACTER TRAP**: A bẫy "tăng weight = tốt hơn". Solution chain: λ→gradient domination→Ĉ≈0→mAP↓. Điểm 22.5/25. Cần embed results |

## Mẹo ra đề

1. Input nên dùng số nhỏ (0, 1, 2) để sinh viên tính tay nhanh
2. Kernel nên có nhiều số 0 để giảm số phép nhân
3. Luôn verify đáp án bằng Python trước khi finalize
4. Đáp án trắc nghiệm câu tính params: dùng "khoảng X%" thay vì số chính xác — tránh sai do làm tròn
5. Khi hỏi giá trị tại vị trí cụ thể: ghi rõ 0-indexed hay 1-indexed
6. Nên chia thành sub-questions thay vì hỏi 1 giá trị duy nhất — giúp chấm điểm từng phần và giảm đoán mò
7. Solution câu lý thuyết: PHẢI giải thích tất cả 4 đáp án (loại trừ), không chỉ đáp án đúng
8. Khi dùng tiêu chí đánh giá trong đề: viết CỤ THỂ, tránh từ trừu tượng ("chuẩn", "chính thức")
9. Hình minh họa output phải thể hiện **đặc trưng riêng** của bài toán — nếu 1 hình có thể interpret nhiều cách thì cần thêm ghi chú
10. Distracter tốt: mô tả **đúng hiện tượng** nhưng **sai nguyên nhân gốc** — buộc sinh viên phân tích sâu, không chỉ nhận diện bề mặt
11. Khi distracter chứa thông tin "gần đúng", solution PHẢI clarify: hiện tượng X là đúng, nhưng nguyên nhân không phải Y mà là Z
12. Bài tính AP/mAP: LUÔN verify bằng Python TRƯỚC KHI finalize — recall chain dễ sai do chia số lẻ
13. Đề thi PHẢI self-contained — nếu cần notebook/link ngoài, ít nhất phải embed data vào đề
14. Distracter tốt nhất: lấy **giá trị trung gian thật** của bài toán (giá trị khi dừng sai bước/nhầm prediction) — bắt sinh viên làm đủ các bước
15. Bài tính nhiều bước: ghi chú round ≥ 6 decimal places ở bước trung gian để tránh sai số tích lũy
16. Bài coding training-dependent: kết quả phụ thuộc GPU/version → nên cho pretrained weights hoặc prediction results thay vì yêu cầu train
17. Diagram/figure PHẢI khớp với LaTeX text — nếu copy ma trận từ channel khác, kiểm tra lại giá trị. Lỗi diagram sai dẫn sinh viên tính nhầm
18. Bài tính AP: PHẢI ghi rõ GT count trong đề — nếu không, sinh viên không biết mẫu số Recall
