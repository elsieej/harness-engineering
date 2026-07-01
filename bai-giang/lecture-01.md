# Bài 01. Mô hình mạnh không có nghĩa là thực thi đáng tin cậy

Bản đầy đủ: [GitHub](https://github.com/walkinglabs/learn-harness-engineering/blob/main/docs/vi/lectures/lecture-01-why-capable-agents-still-fail/index.md) · [Trang web chính thức](https://walkinglabs.github.io/learn-harness-engineering/vi/lectures/lecture-01-why-capable-agents-still-fail/)

## Tóm tắt

Ngay cả các coding agent tốt nhất chỉ đạt 50-60% trên các benchmark đã được chọn lọc kỹ; với tác vụ thực tế mơ hồ, tỷ lệ còn thấp hơn. Anthropic từng chạy cùng một model (Opus 4.5), cùng một prompt, hai lần: chạy trần (20 phút, $9, tính năng cốt lõi không chạy) và chạy có harness đầy đủ với kiến trúc ba agent (6 giờ, $200, sản phẩm chơi được hoàn toàn). Model không đổi — thứ đổi là harness. Các chế độ thất bại phổ biến đều quy về năm nguyên nhân: yêu cầu mơ hồ, quy ước ngầm không được viết ra, môi trường thiết lập dở dang, thiếu cách xác minh, và mất trạng thái giữa các phiên. Nguyên tắc cốt lõi của cả khoá học nằm gọn trong một câu: **khi sự cố xảy ra, đừng đổi model trước — hãy kiểm tra harness trước.**

## Khái niệm chính

- **Khoảng cách năng lực (Capability Gap)**: chênh lệch giữa hiệu suất benchmark và hiệu suất tác vụ thực tế.
- **Harness**: mọi thứ ngoài trọng số model — hướng dẫn, công cụ, môi trường, trạng thái, phản hồi xác minh.
- **Lỗi do harness (Harness-Induced Failure)**: model đủ năng lực nhưng môi trường thực thi có khiếm khuyết cấu trúc.
- **Khoảng cách xác minh (Verification Gap)**: agent tin mình đã xong trong khi thực tế chưa xong.
- **Vòng lặp chẩn đoán (Diagnostic Loop)**: chạy → quan sát lỗi → quy về đúng lớp harness → sửa → chạy lại.
- **Định nghĩa hoàn thành (Definition of Done)**: điều kiện có thể xác minh bằng lệnh (test pass, lint sạch...), chứ không phải cảm giác "trông ổn".

## Bài tập

1. Chọn một codebase bạn quen thuộc và một tác vụ sửa đổi không tầm thường. Chạy agent không có hỗ trợ gì, ghi lại lỗi. Sau đó thêm `AGENTS.md` + lệnh xác minh rõ ràng, chạy lại. Quy mỗi lỗi vào một trong năm lớp phòng thủ (đặc tả, ngữ cảnh, môi trường, xác minh, trạng thái).
2. Chọn 5 tác vụ lập trình. Sau mỗi tác vụ, ghi agent có tự báo "xong" hay không, rồi xác minh độc lập bằng test. Tính tỷ lệ agent báo xong sai — đó là khoảng cách xác minh của bạn.

## Luyện prompt

Bạn sắp giao cho một coding agent (Claude Code, Codex, v.v.) nhiệm vụ "thêm tính năng tìm kiếm" vào một dự án backend bạn tự chọn (thật hoặc tưởng tượng). Hãy viết một **Định nghĩa Hoàn thành (Definition of Done)** rõ ràng cho tác vụ này — không viết prompt chung chung như "thêm tính năng tìm kiếm", mà liệt kê cụ thể: endpoint/giao diện cần có, hành vi mong đợi, các lệnh xác minh phải pass (test, type check, lint). Đây chính là loại artifact mà bài giảng nói sẽ giúp agent không phải "đoán".

Viết câu trả lời của bạn vào `bai-tap-prompt/answers/lecture-01.md` rồi nhờ Claude review.
