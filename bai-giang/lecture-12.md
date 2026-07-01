# Bài 12. Bàn giao sạch ở cuối mỗi phiên

Bản đầy đủ: [GitHub](https://github.com/walkinglabs/learn-harness-engineering/blob/main/docs/vi/lectures/lecture-12-why-every-session-must-leave-a-clean-state/index.md) · [Trang web chính thức](https://walkinglabs.github.io/learn-harness-engineering/vi/lectures/lecture-12-why-every-session-must-leave-a-clean-state/)

## Tóm tắt

Các định luật tiến hoá phần mềm của Lehman: hệ thống thay đổi liên tục tất yếu phức tạp thêm nếu không được quản lý chủ động. Agent copy pattern có sẵn trong repo dù đúng hay sai — nếu không dọn, "rác" tích tụ theo cấp số nhân (ẩn dụ: cốc cà phê để lại trên bàn chung, một tuần sau cả bàn ngập cốc). Trạng thái sạch không chỉ là "build được" — cần đủ 5 điều kiện: build pass, test pass, tiến độ đã ghi vào artifact máy đọc được, không còn artifact tạm (debug log, TODO, code comment-out), và đường khởi động chuẩn vẫn chạy được. Dữ liệu thực nghiệm 12 tuần: không có chiến lược dọn dẹp → build pass rơi từ 100% xuống 68%, thời gian khởi động phiên tăng từ 5 phút lên 60+ phút; có chiến lược dọn dẹp → build vẫn giữ 97%, khởi động chỉ 9 phút. Bài cũng nói về **đơn giản hoá harness định kỳ**: khi model mạnh lên, một số ràng buộc từng cần thiết (ví dụ chia nhỏ sprint cho Sonnet 4.5) trở thành overhead thừa với model mới (Opus 4.6) — nên mỗi tháng thử tắt một thành phần harness, chạy benchmark, giữ/gỡ/thay dựa trên kết quả. Cuối cùng: khi throughput agent vượt xa khả năng review của người, merge nhanh + sửa nhanh có thể tốt hơn cổng chặn nghiêm ngặt — tiêu chí quyết định là so sánh chi phí trung bình sửa 1 bug với chi phí trung bình chờ review.

## Khái niệm chính

- **Trạng thái sạch (Clean State)**: build pass + test pass + tiến độ ghi lại + không artifact cũ + đường khởi động khả dụng.
- **Tính toàn vẹn phiên (Session Integrity)**: giống transaction — hoặc commit đầy đủ + sạch, hoặc rollback, không có nửa vời.
- **Tài liệu chất lượng (Quality Document)**: artifact sống chấm điểm từng module theo thời gian, để biết ưu tiên sửa gì trước.
- **Vòng lặp dọn dẹp hai chế độ**: dọn tức thì (cuối mỗi phiên) + dọn định kỳ (quét toàn hệ thống hàng tuần).
- **Đơn giản hoá harness (Harness Simplification)**: định kỳ gỡ ràng buộc không còn cần thiết khi model mạnh lên.

## Bài tập

1. Thiết kế danh sách kiểm tra thoát phiên bao phủ cả 5 chiều, áp dụng qua 5 phiên liên tiếp, ghi số vi phạm theo từng chiều.
2. Chọn một thành phần harness, tạm vô hiệu hoá, chạy benchmark, so sánh kết quả có/không có nó — quyết định giữ, gỡ hay thay.

## Luyện prompt

Viết một **"Danh sách kiểm tra thoát phiên" (session exit checklist)** đầy đủ 5 mục cho `CLAUDE.md`/`AGENTS.md` của một dự án bạn tự chọn (build/test/tiến độ/artifact tạm/đường khởi động), theo đúng mẫu trong bài giảng. Sau đó viết một đoạn script dọn dẹp giả định (bash pseudocode) có tính **idempotent** (chạy nhiều lần không sinh side effect) để tự động hoá bước dọn artifact tạm.

Viết câu trả lời của bạn vào `bai-tap-prompt/answers/lecture-12.md` rồi nhờ Claude review.

---

Đây là bài giảng cuối cùng. Sau khi hoàn thành cả 12 bài + luyện prompt, hãy chuyển sang Dự án 06 (capstone) trong `../du-an/README.md` để lắp ráp toàn bộ kiến thức thành một harness thật, chạy trên ứng dụng Electron knowledge-base của khoá học.
