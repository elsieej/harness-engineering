# Ví dụ minh hoạ – Dự án 05. Tự xác minh và Phân tách Vai trò

> ⚠️ Số liệu minh hoạ giả định, không phải đo thật. Thay bằng số của bạn khi tự chạy.

## Thiết lập
- [x] Chạy 3 lần trên cùng một tính năng nâng cấp (Q&A trích dẫn chính xác hơn): (1) 1 agent tự làm + tự đánh giá, (2) thêm evaluator độc lập, (3) thêm cả planner

## Kết quả 3 lần chạy (thay cho khung baseline/harness 2 nhánh — dự án này vốn so sánh 3 cấu hình)

| Cấu hình | Thời gian | Vòng lặp | Tỷ lệ pass e2e (đánh giá độc lập bởi người) |
|---|---|---|---|
| (1) 1 agent, tự đánh giá | ~30 phút | 1 | agent tự báo "xong", người kiểm tra lại chỉ đạt 50% tiêu chí thật |
| (2) + evaluator độc lập | ~55 phút | 3 (evaluator từ chối 2 lần trước khi duyệt) | 90% tiêu chí đạt |
| (3) + planner | ~70 phút | 3 | 95% tiêu chí đạt, phạm vi rõ ràng hơn ngay từ đầu |

## Nhận xét
Khác biệt lớn nhất nằm giữa cấu hình (1) và (2) — đúng luận điểm quan trọng nhất bài 09: **tách người làm khỏi người kiểm** mang lại cải thiện lớn hơn nhiều so với thêm planner. Chi phí thời gian tăng gần gấp đôi, nhưng tỷ lệ đạt tiêu chí thật tăng từ 50% lên 90% — đánh đổi hợp lý cho tính năng quan trọng.

## Đối chiếu với `solution/`
_(làm sau khi tự chạy)_
