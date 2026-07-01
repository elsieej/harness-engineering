# Bài 06. Khởi tạo trước mỗi phiên agent

Bản đầy đủ: [GitHub](https://github.com/walkinglabs/learn-harness-engineering/blob/main/docs/vi/lectures/lecture-06-why-initialization-needs-its-own-phase/index.md) · [Trang web chính thức](https://walkinglabs.github.io/learn-harness-engineering/vi/lectures/lecture-06-why-initialization-needs-its-own-phase/)

## Tóm tắt

Khởi tạo (dựng môi trường, cấu hình test, hiểu cấu trúc dự án) và triển khai tính năng có mục tiêu tối ưu khác nhau về bản chất. Khi trộn lẫn, agent tự nhiên ưu tiên viết code (giá trị thấy ngay) và làm hạ tầng cho có, dẫn tới: hạ tầng không được xây tử tế, "tích tụ chưa xác minh" (code viết trước khi có test), lãng phí ngân sách ngữ cảnh ở cả hai đầu, và bãi mìn giả định ngầm (quyết định khởi tạo không ghi lại, phiên sau mâu thuẫn — ví dụ phiên 1 chọn Vitest, phiên 2 không biết lại thêm Jest). Giải pháp: coi khởi tạo là **giai đoạn riêng biệt**, phiên đầu tiên không viết code nghiệp vụ, chỉ tạo ra: môi trường chạy được, khung test đã xác minh (ít nhất 1 test pass), tài liệu "danh sách sẵn sàng" (startup readiness checklist), bản phân rã tác vụ, và một git commit checkpoint sạch. Nên khởi động từ template có sẵn thay vì từ thư mục rỗng. Dữ liệu của Anthropic: dự án có giai đoạn khởi tạo riêng đạt tỷ lệ hoàn thành tính năng cao hơn 31% trong kịch bản đa phiên.

## Khái niệm chính

- **Giai đoạn khởi tạo (Initialization Phase)**: chỉ tạo hạ tầng, không phát triển tính năng.
- **Danh sách kiểm tra sẵn sàng (Startup Readiness Checklist)**: chạy được + test được + thấy tiến độ được + chọn được bước tiếp theo.
- **Từ đầu vs. từ mẫu (From Scratch vs. From Template)**: khởi động từ template luôn tốt hơn.
- **Luôn sẵn sàng bàn giao (Always Ready to Hand Off)**: dự án ở bất kỳ thời điểm nào cũng có thể được một agent mới tiếp quản chỉ bằng cách đọc repo.
- **Thời gian tới test pass đầu tiên**: chỉ số cốt lõi đo hiệu quả khởi tạo.

## Bài tập

1. Viết một danh sách sẵn sàng hoàn chỉnh cho dự án của bạn. Mở phiên agent hoàn toàn mới, chỉ cho xem repo, để nó khởi động/chạy test/hiểu tiến độ. Ghi lại vấn đề gặp phải — mỗi vấn đề là một điều khoản còn thiếu.
2. So sánh hai cách trên một dự án mới: (A) agent vừa khởi tạo vừa làm tính năng đầu; (B) dành hẳn 1 phiên cho khởi tạo. Sau 4 phiên, so sánh thời gian tới test pass đầu tiên, chi phí tái thiết lập, tỷ lệ hoàn thành.

## Luyện prompt

Viết một prompt "chỉ khởi tạo" mà bạn sẽ giao cho agent ở phiên đầu tiên của một dự án mới (tự chọn: ví dụ một REST API nhỏ). Prompt phải **cấm rõ ràng** việc viết code tính năng nghiệp vụ, và yêu cầu agent tạo đủ 4 thứ: môi trường chạy được, 1 test mẫu pass, tài liệu danh sách sẵn sàng, và bản phân rã tác vụ thành ít nhất 3 mục có tiêu chí chấp nhận. Kèm theo danh sách chấp nhận (acceptance checklist) mà bạn sẽ dùng để xác nhận phiên khởi tạo đã đạt.

Viết câu trả lời của bạn vào `bai-tap-prompt/answers/lecture-06.md` rồi nhờ Claude review.
