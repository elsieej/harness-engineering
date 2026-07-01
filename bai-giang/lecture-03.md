# Bài 03. Biến kho lưu trữ thành nguồn sự thật duy nhất

Bản đầy đủ: [../../learn-harness-engineering/docs/vi/lectures/lecture-03-why-the-repository-must-become-the-system-of-record/index.md](../../learn-harness-engineering/docs/vi/lectures/lecture-03-why-the-repository-must-become-the-system-of-record/index.md)

## Tóm tắt

Agent chỉ có ba nguồn đầu vào: system prompt/mô tả tác vụ, nội dung tệp trong repo, và kết quả thực thi công cụ. Slack, Jira, Confluence, hay câu chuyện bên lề — agent không hề thấy. Nguyên tắc "repo là spec" của OpenAI nghĩa là: **thông tin không nằm trong repo thì không tồn tại với agent.** Bài giới thiệu "bài kiểm tra phiên mới" (fresh session test): mở phiên hoàn toàn mới, chỉ cho xem repo, xem nó có trả lời được 5 câu hỏi cơ bản không (Hệ thống này là gì? Tổ chức ra sao? Chạy thế nào? Xác minh thế nào? Tiến độ hiện tại ra sao?). Nếu không, bản đồ của bạn có lỗ hổng. Bài cũng áp dụng ẩn dụ ACID (Atomicity, Consistency, Isolation, Durability) vào quản lý trạng thái agent: mỗi thao tác logic gói trong một commit, có vị từ "nhất quán" rõ ràng, tránh race condition khi nhiều agent chạy song song, và kiến thức quan trọng phải được ghi vào tệp git-tracked (không chỉ nằm trong đầu người).

## Khái niệm chính

- **Khoảng cách hiển thị kiến thức (Knowledge Visibility Gap)**: tỷ lệ kiến thức dự án KHÔNG nằm trong repo.
- **Hệ thống ghi chép (System of Record)**: repo là nguồn có thẩm quyền cuối cùng, không nơi nào khác tính.
- **Bài kiểm tra phiên mới (Fresh Session Test)**: 5 câu hỏi để đánh giá "bản đồ" có đủ tốt không.
- **Chi phí khám phá (Discovery Cost)**: ngân sách ngữ cảnh agent tốn để tìm thông tin quan trọng bị giấu kỹ.
- **Tốc độ suy giảm kiến thức (Knowledge Decay Rate)**: tài liệu lệch khỏi code còn nguy hiểm hơn không có tài liệu.
- **ACID cho trạng thái agent**: nguyên tử, nhất quán, cô lập, bền vững.

## Bài tập

1. Chạy bài kiểm tra phiên mới trên dự án của bạn: mở phiên agent mới, chỉ cho xem repo, hỏi 5 câu. Ghi lại câu nào không trả lời được, cải thiện repo đến khi trả lời hết.
2. Liệt kê mọi quyết định/ràng buộc quan trọng của dự án, đánh dấu cái nào đã nằm trong repo. Tính khoảng cách hiển thị kiến thức, đặt mục tiêu đưa xuống dưới 10%.

## Luyện prompt

Chọn một quyết định kiến trúc "ai cũng biết nhưng chưa viết ra" trong một dự án bạn từng làm (hoặc tưởng tượng một cái hợp lý, ví dụ "mọi truy vấn DB phải dùng transaction"). Viết nó thành một đoạn `ARCHITECTURE.md` hoặc `CONSTRAINTS.md` ngắn (dùng ngôn ngữ PHẢI/KHÔNG ĐƯỢC rõ ràng), đặt đúng vị trí cạnh code liên quan, sao cho một agent mới đọc repo là hiểu ngay ràng buộc mà không cần hỏi ai.

Viết câu trả lời của bạn vào `bai-tap-prompt/answers/lecture-03.md` rồi nhờ Claude review.
