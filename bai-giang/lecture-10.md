# Bài 10. Chỉ chạy toàn bộ pipeline mới tính là xác minh thật sự

Bản đầy đủ: [GitHub](https://github.com/walkinglabs/learn-harness-engineering/blob/main/docs/vi/lectures/lecture-10-why-end-to-end-testing-changes-results/index.md) · [Trang web chính thức](https://walkinglabs.github.io/learn-harness-engineering/vi/lectures/lecture-10-why-end-to-end-testing-changes-results/)

## Tóm tắt

Từng component có thể pass unit test riêng lẻ nhưng vẫn sai khi ghép lại: không khớp giao diện (renderer gửi path tương đối, preload kỳ vọng path tuyệt đối — cả hai test đều mock nên đều pass), lỗi lan truyền trạng thái, vấn đề vòng đời tài nguyên (rò rỉ file handle), phụ thuộc môi trường. Điểm thú vị của bài này: kiểm thử end-to-end **không chỉ phát hiện lỗi, mà thay đổi cách agent viết code ngay từ đầu** — khi biết sẽ bị test toàn bộ pipeline, agent bắt đầu cân nhắc tương tác giữa component, tôn trọng ranh giới kiến trúc, xử lý đường lỗi kỹ hơn. OpenAI khuyến nghị: với codebase do agent tạo, **ràng buộc kiến trúc phải là điều kiện tiên quyết từ ngày đầu** (ví dụ: kiến trúc phân lớp Types → Config → Repo → Service → Runtime → UI, phụ thuộc chỉ chảy một chiều), vì agent có xu hướng copy pattern sẵn có dù đúng hay sai. Mỗi ràng buộc kiến trúc nên có kiểm tra tự động tương ứng (lint script), và mỗi lần phát hiện lỗi lặp lại trong review, hãy "thăng cấp" nó thành test tự động — đây gọi là Review Feedback Promotion. Thông báo lỗi luôn cần 3 phần: sai ở đâu, vì sao, sửa thế nào.

## Khái niệm chính

- **Khiếm khuyết ở ranh giới component**: cả hai bên pass test riêng nhưng tương tác sai.
- **Gradient đầy đủ kiểm thử**: unit ⊆ integration ⊆ end-to-end về khả năng phát hiện lỗi.
- **Quy tắc thực thi ranh giới kiến trúc**: biến quy tắc kiến trúc từ tài liệu thành lint check tự động.
- **Thăng cấp phản hồi review (Review Feedback Promotion)**: comment lặp lại trong review → trở thành test tự động vĩnh viễn.
- **Thông báo lỗi hướng agent**: luôn kèm "cách sửa cụ thể", không chỉ "sai rồi".

## Bài tập

1. Chọn một tác vụ chạm ≥3 component. Chạy unit test trước, ghi kết quả, rồi chạy end-to-end. Phân loại mỗi lỗi phát hiện thêm theo nhóm vấn đề tương tác nào.
2. Chọn một ràng buộc kiến trúc trong dự án của bạn, biến nó thành một kiểm tra tự động (kèm thông báo lỗi hướng agent), tích hợp vào harness.

## Luyện prompt

Viết một quy tắc kiến trúc "PHẢI/KHÔNG ĐƯỢC" cho dự án bạn tự chọn (ví dụ: "renderer process không được truy cập trực tiếp filesystem"), rồi viết một đoạn script/kiểm tra giả định (pseudocode `grep`/lint) thực thi ràng buộc đó, kèm thông báo lỗi đủ 3 phần (sai ở đâu / vì sao / sửa thế nào) theo đúng mẫu trong bài giảng.

Viết câu trả lời của bạn vào `bai-tap-prompt/answers/lecture-10.md` rồi nhờ Claude review.
