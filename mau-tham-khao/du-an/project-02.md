# Ví dụ minh hoạ – Dự án 02. Không gian làm việc Agent đọc được

> ⚠️ Số liệu minh hoạ giả định, không phải đo thật. Thay bằng số của bạn khi tự chạy.

## Thiết lập
- [x] Bản B thêm `ARCHITECTURE.md`, `PRODUCT.md`, `session-handoff.md` cạnh code liên quan (không phải một tài liệu tổng ở root)

## Nhánh A — Baseline
- Thời gian: ~35 phút cho "thêm trang chi tiết tài liệu"
- Số vòng lặp: 2 (agent đoán sai layout dữ liệu 1 lần, phải sửa lại)
- Tỷ lệ pass e2e: 2/3 (thiếu xử lý khi tài liệu không tồn tại)
- Quan sát: agent hỏi lại người dùng 3 lần để làm rõ cấu trúc dữ liệu — dấu hiệu "khoảng cách hiển thị kiến thức" ở bài 03

## Nhánh B — Có harness
- Thời gian: ~40 phút (chậm hơn không đáng kể so với A)
- Số vòng lặp: 1
- Tỷ lệ pass e2e: 3/3
- Quan sát: agent tự tìm thấy convention xử lý lỗi trong `ARCHITECTURE.md`, không hỏi lại lần nào — bài kiểm tra phiên mới (bài 03) coi như đạt

## So sánh & nhận xét
Khác với dự án 01 (chênh lệch thời gian lớn), ở đây B gần như không chậm hơn A — vì chi phí đọc thêm 2 tài liệu ngắn nhỏ hơn nhiều chi phí hỏi lại/đoán sai. Đây đúng là luận điểm "tài liệu kiến trúc đặt cạnh code" của bài 03-04: chi phí thấp, lợi ích cao khi tác vụ chạm nhiều giả định ngầm.

## Đối chiếu với `solution/`
_(làm sau khi tự chạy)_
