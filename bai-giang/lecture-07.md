# Bài 07. Vạch ranh giới tác vụ rõ ràng cho agent

Bản đầy đủ: [GitHub](https://github.com/walkinglabs/learn-harness-engineering/blob/main/docs/vi/lectures/lecture-07-why-agents-overreach-and-under-finish/index.md) · [Trang web chính thức](https://walkinglabs.github.io/learn-harness-engineering/vi/lectures/lecture-07-why-agents-overreach-and-under-finish/)

## Tóm tắt

Giao một yêu cầu rộng ("thêm xác thực người dùng") và agent thường lan man: sửa schema, viết route, đổi frontend, "tiện thể" tái cấu trúc middleware, sắp xếp lại thư mục test. Hai tiếng sau: nhiều tệp bị sửa, nhiều dòng code mới, nhưng chẳng tính năng nào chạy end-to-end. Bản chất là toán học tài nguyên: nếu ngân sách ngữ cảnh C bị chia cho k tác vụ đồng thời, mỗi tác vụ chỉ nhận C/k, và khi C/k dưới ngưỡng tối thiểu thì không cái nào xong. Dữ liệu Anthropic: agent dùng chiến lược "bước tiếp theo nhỏ" (WIP=1) có tỷ lệ hoàn thành cao hơn 37% so với prompt rộng; số dòng code tương quan ÂM với tỷ lệ hoàn thành thực tế — viết nhiều hơn thường đồng nghĩa hoàn thành ít hơn. Giải pháp cốt lõi: **thực thi giới hạn WIP=1** (chỉ một tác vụ "active" tại một thời điểm), định nghĩa **bằng chứng hoàn thành** phải là hành vi thực thi được xác minh (không phải "code trông ổn"), đưa **bề mặt phạm vi** ra một tệp máy đọc được, và theo dõi VCR (Verified Completion Rate) = số tác vụ đã xác minh / số tác vụ đã kích hoạt.

## Khái niệm chính

- **Vượt phạm vi (Overreach)**: kích hoạt nhiều tác vụ hơn mức tối ưu trong một phiên.
- **Dở dang (Under-finish)**: tỷ lệ tác vụ pass xác minh end-to-end thấp so với số đã kích hoạt.
- **Giới hạn WIP (Work-in-Progress Limit)**: mượn từ Kanban; WIP=1 là mặc định an toàn cho agent.
- **Bằng chứng hoàn thành (Completion Evidence)**: điều kiện xác minh được, không phải cảm giác.
- **Bề mặt phạm vi (Scope Surface)**: DAG các đơn vị công việc, mỗi nút có 4 trạng thái (not_started/active/blocked/passing).
- **VCR (Verified Completion Rate)**: chỉ số theo dõi liên tục để chặn việc mở tác vụ mới khi VCR < 1.0.

## Bài tập

1. Phân rã một yêu cầu rộng (ví dụ "triển khai hệ thống quản lý người dùng") thành ≥5 đơn vị nguyên tử, mỗi đơn vị có: hành vi, lệnh xác minh, phụ thuộc. Kiểm tra có thoả WIP=1 không.
2. Chạy cùng một dự án hai lần: không ràng buộc vs. WIP=1 được thực thi. So sánh tỷ lệ hoàn thành đã xác minh và tổng số dòng code.

## Luyện prompt

Viết đoạn quy tắc "Quy tắc làm việc" cho `CLAUDE.md`/`AGENTS.md` thực thi WIP=1 cho một dự án bạn tự chọn, và kèm theo 3 mục feature-list mẫu ở định dạng có đủ 3 trường (id, lệnh xác minh thực thi được như `curl ... | jq ...`, trạng thái). Mục tiêu: khi agent đọc xong, nó chỉ có thể làm một việc tại một thời điểm và không thể tự nhận "xong" mà không qua lệnh xác minh.

Viết câu trả lời của bạn vào `bai-tap-prompt/answers/lecture-07.md` rồi nhờ Claude review.
