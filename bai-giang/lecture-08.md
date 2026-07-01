# Bài 08. Sử dụng feature list để ràng buộc những gì agent làm

Bản đầy đủ: [GitHub](https://github.com/walkinglabs/learn-harness-engineering/blob/main/docs/vi/lectures/lecture-08-why-feature-lists-are-harness-primitives/index.md) · [Trang web chính thức](https://walkinglabs.github.io/learn-harness-engineering/vi/lectures/lecture-08-why-feature-lists-are-harness-primitives/)

## Tóm tắt

Agent không tự biết "xong" nghĩa là gì — nếu bạn không định nghĩa, nó sẽ tự lấy chuẩn ngầm "code trông khá đầy đủ". Feature list không phải ghi chú cho người đọc, mà là **cấu trúc nền (primitive)** mà cả harness dựa vào: bộ lập lịch đọc nó để chọn tác vụ tiếp theo, bộ xác minh đọc nó để phán xét hoàn thành, trình báo cáo bàn giao dùng nó để sinh tóm tắt. Mỗi mục tính năng cần đủ **bộ ba**: mô tả hành vi, lệnh xác minh, trạng thái (`not_started` / `active` / `blocked` / `passing`). Quan trọng: **chuyển trạng thái phải do harness kiểm soát** (giống ràng buộc trigger ở tầng database, agent không lách được), agent không được tự ý đánh dấu "passing" — chỉ lệnh xác minh chạy pass mới cho phép chuyển. Một nhóm thương mại điện tử so sánh: theo dõi bằng ghi chú tự do vs. feature list có cấu trúc — kết quả feature list có cấu trúc đạt tỷ lệ hoàn thành cao hơn 45%, và 0 lần triển khai trùng lặp.

## Khái niệm chính

- **Feature list là nguyên thuỷ của harness**: không phải tài liệu tuỳ chọn, mà cấu trúc dữ liệu mọi thành phần khác phụ thuộc vào.
- **Cấu trúc ba trường**: (mô tả hành vi, lệnh xác minh, trạng thái) — thiếu một là chưa hoàn chỉnh.
- **Máy trạng thái 4 giá trị**: `not_started` / `active` / `blocked` / `passing`.
- **Cổng trạng thái pass (Pass-state gating)**: chuyển sang `passing` chỉ qua lệnh xác minh chạy thành công, một chiều.
- **Hiệu chỉnh độ hạt (Granularity)**: mỗi mục nên "hoàn thành được trong một phiên" — không quá rộng, không quá vụn.

## Bài tập

1. Thiết kế một JSON schema feature list tối giản (id, hành vi, lệnh xác minh, trạng thái, bằng chứng), áp dụng cho một dự án thật với 5 tính năng.
2. Chọn 3 tính năng, thiết kế cả xác minh "lỏng" và xác minh "nghiêm", so sánh tỷ lệ dương tính giữa hai cách.

## Luyện prompt

Chọn một tính năng ở mức "quá rộng" (ví dụ "triển khai giỏ hàng") và một ở mức "quá hẹp" (ví dụ "tạo trường tên trên Cart model"). Viết lại cả hai thành các mục feature-list ở đúng độ hạt ("hoàn thành được trong một phiên"), mỗi mục kèm lệnh xác minh cụ thể (ví dụ dùng `curl` + `jq`). Sau đó viết đoạn quy tắc ngắn cho `CLAUDE.md` nói rõ: agent không được tự sửa trạng thái trong feature list, chỉ script xác minh mới được cập nhật.

Viết câu trả lời của bạn vào `bai-tap-prompt/answers/lecture-08.md` rồi nhờ Claude review.
