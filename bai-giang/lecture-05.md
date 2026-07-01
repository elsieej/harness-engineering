# Bài 05. Duy trì ngữ cảnh xuyên suốt các phiên

Bản đầy đủ: [../../learn-harness-engineering/docs/vi/lectures/lecture-05-why-long-running-tasks-lose-continuity/index.md](../../learn-harness-engineering/docs/vi/lectures/lecture-05-why-long-running-tasks-lose-continuity/index.md)

## Tóm tắt

Cửa sổ ngữ cảnh luôn hữu hạn — không phải vấn đề nâng cấp model là giải quyết được. Vấn đề sâu hơn: thông tin agent tạo ra không đồng đều tầm quan trọng — các bước lý luận trung gian chứa "vì sao" của quyết định, nhưng kết quả cuối chỉ giữ "cái gì". Nén (compaction) hay mất "vì sao"; đặt lại (reset) thì sạch nhưng phụ thuộc vào độ đầy đủ của artifact đã lưu. Anthropic quan sát hiện tượng "lo lắng ngữ cảnh" (context anxiety): khi ngữ cảnh sắp cạn, agent vội vàng kết thúc, bỏ qua xác minh. Thú vị là: Sonnet 4.5 cần reset ngữ cảnh làm cốt lõi thiết kế harness, còn Opus 4.5 quản lý tốt hơn chỉ với compaction — nghĩa là **thiết kế harness phải hiểu rõ model mục tiêu, không dùng chung một khuôn**. Giải pháp thực tế: đối xử với agent như một kỹ sư bị xoá bộ nhớ ngắn hạn mỗi phiên — dùng PROGRESS.md (trạng thái/đã xong/đang làm/bước tiếp theo), DECISIONS.md (quyết định + lý do + phương án bị loại), git commit làm checkpoint, và quy trình "vào ca/tan ca" rõ ràng trong AGENTS.md.

## Khái niệm chính

- **Chi phí tái thiết lập (Rebuild Cost)**: thời gian phiên mới cần để vào trạng thái làm việc được — mục tiêu: dưới 3 phút.
- **Trôi dạt (Drift)**: khoảng cách giữa hiểu biết của agent và trạng thái thật của repo, tích luỹ qua từng phiên nếu không kiểm soát.
- **Lo lắng ngữ cảnh (Context Anxiety)**: hành vi "vội vàng kết thúc" khi ngữ cảnh sắp cạn.
- **Nén vs. Đặt lại (Compaction vs. Reset)**: hai chiến lược, mỗi cái có đánh đổi khác nhau; chọn theo model cụ thể.
- **PROGRESS.md / DECISIONS.md**: hai công cụ nền tảng cho tính liên tục đa phiên.

## Bài tập

1. Chọn một tác vụ cần ≥3 phiên. Không dùng tệp trạng thái nào, đo chi phí tái thiết lập mỗi phiên. Sau đó thêm PROGRESS.md và lặp lại, so sánh.
2. Thiết kế một mẫu bàn giao tối giản (4 trường: trạng thái repo, trạng thái runtime, chướng ngại, hành động kế tiếp). Thử để một phiên hoàn toàn mới khôi phục chỉ dựa vào mẫu này.

## Luyện prompt

Viết một mục **PROGRESS.md** thực tế cho một tính năng đang làm dở (tự chọn ngữ cảnh, ví dụ "thêm phân trang cho API danh sách sản phẩm"), bao gồm đủ 4 phần: Trạng thái hiện tại (commit, test pass/fail), Đã hoàn thành, Đang thực hiện, Vấn đề đã biết, Các bước tiếp theo. Sau đó viết thêm một mục **DECISIONS.md** ghi lại MỘT quyết định thiết kế giả định trong tác vụ đó kèm lý do và phương án bị loại. Đây là bài luyện viết đúng loại "bàn giao" mà một agent phiên sau cần để tiếp tục trong dưới 3 phút.

Viết câu trả lời của bạn vào `bai-tap-prompt/answers/lecture-05.md` rồi nhờ Claude review.
