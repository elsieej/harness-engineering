# Ví dụ minh hoạ – Dự án 03. Tính liên tục đa phiên

> ⚠️ Số liệu minh hoạ giả định, không phải đo thật. Thay bằng số của bạn khi tự chạy.

## Thiết lập
- [x] Tác vụ chọn: document chunking + metadata extraction + Q&A có trích dẫn, chia làm 3 phiên bắt buộc (đóng app giữa các phiên)

## Nhánh A — Baseline (không PROGRESS.md/feature list)
- Thời gian: phiên 1: 30 phút; phiên 2: +25 phút "tái thiết lập" (đọc lại code đoán tiến độ); phiên 3: +30 phút, và phát hiện phiên 2 đã đánh dấu nhầm 1 tính năng "xong" dù chưa xong (không có script xác minh chặn)
- Tỷ lệ pass e2e cuối cùng: 2/3 tính năng

## Nhánh B — Có harness (feature_list.json + claude-progress.md, trạng thái do harness kiểm soát)
- Thời gian: phiên 1: 35 phút; phiên 2: +5 phút tái thiết lập (đọc progress file); phiên 3: +5 phút
- Tỷ lệ pass e2e cuối cùng: 3/3, không có tính năng nào bị đánh dấu nhầm

## So sánh & nhận xét
Chênh lệch rõ nhất nằm ở **chi phí tái thiết lập** giữa các phiên (bài 05: mục tiêu <3 phút) — nhánh B gần đạt, nhánh A tốn 25-30 phút mỗi lần, và quan trọng hơn: nhánh A có 1 lần "trôi dạt" trạng thái (tính năng bị đánh dấu xong sai) — đúng rủi ro mà bài 08 cảnh báo khi agent tự sửa trạng thái thay vì để script xác minh làm việc đó.

## Đối chiếu với `solution/`
_(làm sau khi tự chạy)_
