# Ví dụ minh hoạ – Dự án 04. Phản hồi Runtime và Kiểm soát Phạm vi

> ⚠️ Số liệu minh hoạ giả định, không phải đo thật. Thay bằng số của bạn khi tự chạy.

## Thiết lập
- [x] Bản B thêm log khởi động/import/indexing + lint kiến trúc chống vi phạm layer + feature list làm cổng phạm vi
- [x] Cả 2 bản đều được cài sẵn 1 lỗi runtime giống nhau (theo đề bài dự án) để xem agent tự sửa thế nào

## Nhánh A — Baseline
- Thời gian: ~50 phút, phần lớn dành để agent "đoán mù" tìm lỗi vì không có log
- Số vòng lặp: 5 (thử sai nhiều lần không có tín hiệu)
- Tỷ lệ pass e2e: sửa được lỗi cài sẵn nhưng vô tình sửa luôn 1 file không liên quan (vượt phạm vi — overreach, đúng chủ đề bài 07)

## Nhánh B — Có harness
- Thời gian: ~25 phút
- Số vòng lặp: 2 (log runtime chỉ thẳng vị trí lỗi ở lần thử đầu)
- Tỷ lệ pass e2e: sửa đúng phạm vi, không đụng file ngoài feature đang active (WIP=1 được tôn trọng)

## So sánh & nhận xét
Đây là dự án cho thấy rõ nhất tác động của **observability** (bài 11) lồng vào **kiểm soát phạm vi** (bài 07-08): có log → tìm lỗi nhanh hơn hẳn; có feature-list gating → không lan man sửa việc ngoài phạm vi dù có "tiện tay" sửa được.

## Đối chiếu với `solution/`
_(làm sau khi tự chạy)_
