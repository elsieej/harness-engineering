# Bài 02. Harness thực sự có nghĩa là gì

Bản đầy đủ: [GitHub](https://github.com/walkinglabs/learn-harness-engineering/blob/main/docs/vi/lectures/lecture-02-what-a-harness-actually-is/index.md) · [Trang web chính thức](https://walkinglabs.github.io/learn-harness-engineering/vi/lectures/lecture-02-what-a-harness-actually-is/)

## Tóm tắt

Nhiều người dùng từ "harness" nhưng thực ra chỉ đang nói về một tệp prompt — điều đó chưa đủ. Bài này định nghĩa harness gồm **năm hệ thống phụ**: Hướng dẫn (AGENTS.md/CLAUDE.md), Công cụ (shell, file access, test runner), Môi trường (dependency lock, version pin, container), Trạng thái (PROGRESS.md, git history), và Phản hồi (lệnh xác minh: test/lint/type-check). Thiếu bất kỳ cái nào, harness coi như chưa trọn vẹn. Một câu chuyện thật minh hoạ: nhóm dùng GPT-4o phát triển frontend qua 4 giai đoạn, mỗi giai đoạn thêm một hệ thống phụ, tỷ lệ thành công đi từ 20% lên gần 100% — không đổi model, chỉ đổi harness. Bài cũng giới thiệu kỹ thuật "loại trừ biến có kiểm soát" (ablation): gỡ từng hệ thống phụ để đo mức đóng góp biên của nó, nhưng lưu ý: kết quả ablation chỉ là bằng chứng hỗ trợ, muốn tìm điểm nghẽn thật vẫn phải dựa vào nhật ký lỗi và quy kết nguyên nhân.

## Khái niệm chính

- **Năm hệ thống phụ**: Hướng dẫn + Công cụ + Môi trường + Trạng thái + Phản hồi.
- **Repo là nguồn sự thật duy nhất**: agent không thấy gì ngoài repo, tool output, và task description.
- **"Đưa bản đồ, đừng đưa cẩm nang"**: AGENTS.md nên là trang thư mục ngắn (~100 dòng), không phải bách khoa toàn thư.
- **Ràng buộc, đừng vi quản lý**: dùng quy tắc thực thi được (executable invariants) thay vì liệt kê chi tiết từng bước.
- **Thí nghiệm loại trừ biến có kiểm soát**: gỡ từng hệ thống phụ, đo mức giảm hiệu suất, để biết thành phần nào đang có giá trị nhất — không phải để xác định điểm nghẽn tuyệt đối.

## Bài tập

1. Kiểm toán một dự án bạn đang dùng AI agent theo khung năm hệ thống phụ, chấm điểm 1-5 từng cái. Cải thiện hệ thống phụ điểm thấp nhất trong 30 phút, quan sát thay đổi.
2. Chọn một tác vụ, lần lượt gỡ hướng dẫn / phản hồi / trạng thái (mỗi lần một thứ), đo mức giảm hiệu suất để xếp hạng giá trị biên.

## Luyện prompt

Viết một `AGENTS.md` (hoặc `CLAUDE.md`) mẫu cho một dự án bạn tự chọn, phủ đủ 5 hệ thống phụ trong khuôn khổ dưới 100 dòng: tổng quan dự án, lệnh khởi động, ràng buộc cứng (tối đa 10-15 điều), lệnh xác minh, và (nếu cần) liên kết tới tài liệu chủ đề chi tiết hơn. Mục tiêu là luyện viết đúng "tầm cao" — đủ để một agent mới hoàn toàn không hỏi bạn cũng bắt tay làm việc được.

Viết câu trả lời của bạn vào `bai-tap-prompt/answers/lecture-02.md` rồi nhờ Claude review.
