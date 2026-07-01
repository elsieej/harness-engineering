# Bài 11. Làm cho runtime của agent có thể quan sát được

Bản đầy đủ: [../../learn-harness-engineering/docs/vi/lectures/lecture-11-why-observability-belongs-inside-the-harness/index.md](../../learn-harness-engineering/docs/vi/lectures/lecture-11-why-observability-belongs-inside-the-harness/index.md)

## Tóm tắt

Không có khả năng quan sát, mọi quyết định của agent thành phỏng đoán: không phân biệt được "đúng" với "trông có vẻ đúng", đánh giá trở thành huyền bí (mỗi evaluator dựa vào giả định ngầm khác nhau), thử lại thành đoán mù, và bàn giao phiên mất 30-50% thời gian chỉ để chẩn đoán lại. Bài giới thiệu **quan sát phân lớp**: quan sát runtime (log, trace, health check — trả lời "hệ thống đã làm gì") và quan sát quá trình (sprint contract, rubric chấm điểm — trả lời "vì sao thay đổi này nên được chấp nhận"). Thí nghiệm ba-agent của Anthropic (planner mở rộng yêu cầu → generator triển khai từng sprint theo "sprint contract" đã thoả thuận trước → evaluator dùng Playwright MCP tương tác thật với ứng dụng, chấm theo rubric 4 chiều) cho kết quả chất lượng cao hơn nhiều so với không có quan sát (15 phút/1 vòng vs. 45 phút/3-4 vòng lặp mù). Quan trọng: agent không tự giải quyết được vấn đề này bằng cách "tự in log", vì nó không biết nó không biết cái gì cần log, và quan sát quá trình (sprint contract, rubric) là artifact cấu trúc cần hỗ trợ ở cấp harness, không phải print statement.

## Khái niệm chính

- **Quan sát runtime vs. quan sát quá trình**: "hệ thống đã làm gì" vs. "vì sao chấp nhận thay đổi này".
- **Task trace**: bản ghi đầy đủ đường đi quyết định của agent, giống distributed tracing.
- **Sprint contract**: thoả thuận trước khi code — phạm vi, tiêu chuẩn xác minh, ngoại lệ rõ ràng.
- **Evaluator rubric**: chấm điểm có cấu trúc dựa trên bằng chứng, để nhiều evaluator ra kết luận tương tự.
- **Sự thật quan trọng**: evaluator cũng cần được tinh chỉnh liên tục — đọc log evaluator, tìm chỗ nó lệch khỏi phán đoán người, cập nhật prompt QA.

## Bài tập

1. Kiểm toán harness hiện tại của bạn theo hai lớp quan sát (hệ thống + quá trình). Tìm trạng thái hệ thống không phân biệt được từ tín hiệu hiện có.
2. Viết một sprint contract cho một tác vụ thật, để agent thực thi theo, so sánh hiệu quả/chất lượng với không có contract.

## Luyện prompt

Viết một **sprint contract** hoàn chỉnh (theo mẫu: Phạm vi / Tiêu chuẩn xác minh / Ngoại lệ) cho một tính năng bạn tự chọn (ví dụ "thêm chế độ tối cho ứng dụng"). Sau đó viết một **rubric chấm điểm** 3-4 chiều (mỗi chiều có 4 mức A-D với tiêu chí cụ thể) mà một evaluator độc lập sẽ dùng để chấm sprint đó — đảm bảo rubric buộc dùng bằng chứng cụ thể (số đo, log, ảnh chụp), không chấp nhận nhận định mơ hồ kiểu "trông ổn".

Viết câu trả lời của bạn vào `bai-tap-prompt/answers/lecture-11.md` rồi nhờ Claude review.
