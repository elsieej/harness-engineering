# Ví dụ minh hoạ – Dự án 06. Harness Đầy đủ (Capstone)

> ⚠️ Số liệu minh hoạ giả định, không phải đo thật. Thay bằng số của bạn khi tự chạy.

## Thiết lập
- [x] Chạy benchmark 3 lần: harness yếu (chỉ AGENTS.md ngắn) → harness mạnh nhất (đủ 5 hệ thống phụ + observability + clean-state checklist) → dọn dẹp → chạy lại → thí nghiệm ablation

## Kết quả benchmark chính

| Cấu hình | Tỷ lệ tính năng pass e2e | Thời gian tổng | Ghi chú |
|---|---|---|---|
| Harness yếu | 40% | ~1.5 giờ | Nhiều lần tuyên bố xong sớm, không bị chặn |
| Harness đầy đủ | 95% | ~4 giờ | Chậm hơn đáng kể nhưng ổn định qua nhiều phiên |

## Thí nghiệm ablation (gỡ từng thành phần khỏi harness đầy đủ)

| Gỡ thành phần | Tỷ lệ pass e2e còn lại | Kết luận |
|---|---|---|
| Gỡ feature-list gating | 65% | Giảm mạnh — thành phần quan trọng nhất |
| Gỡ evaluator độc lập | 70% | Giảm mạnh — đúng thứ 2 |
| Gỡ observability/log runtime | 80% | Giảm vừa — chủ yếu ảnh hưởng tốc độ sửa lỗi, ít ảnh hưởng tỷ lệ pass cuối |
| Gỡ session exit checklist | 90% (đo ở phiên đầu) nhưng giảm dần qua 5 phiên tiếp theo | Ảnh hưởng tích luỹ theo thời gian, không thấy ngay ở 1 phiên |

## Nhận xét
Ablation xác nhận trực giác xây dựng từ bài 01-11: **cổng trạng thái (feature-list) và tách vai trò (evaluator)** là 2 thành phần có giá trị biên cao nhất trong tình huống này — khớp với nhấn mạnh của bài 08 và 09. Session exit checklist (bài 12) không thấy tác động ngay nhưng đúng như bài giảng cảnh báo, ảnh hưởng của nó là tích luỹ — cần đo qua nhiều phiên mới thấy rõ, một benchmark 1-lần sẽ đánh giá thấp giá trị của nó.

## Đối chiếu với `solution/`
_(làm sau khi tự chạy — đây là dự án capstone, nên so sánh kỹ đặc biệt phần kiến trúc 3-agent planner/generator/evaluator)_
