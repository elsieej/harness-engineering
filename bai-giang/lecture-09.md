# Bài 09. Ngăn chặn agent tuyên bố thành công quá sớm

Bản đầy đủ: [GitHub](https://github.com/walkinglabs/learn-harness-engineering/blob/main/docs/vi/lectures/lecture-09-why-agents-declare-victory-too-early/index.md) · [Trang web chính thức](https://walkinglabs.github.io/learn-harness-engineering/vi/lectures/lecture-09-why-agents-declare-victory-too-early/)

## Tóm tắt

Nghiên cứu ICML 2017 (Guo et al.) chứng minh mạng nơ-ron hiện đại tự tin thái quá một cách có hệ thống — AI coding agent cũng vậy. Agent viết code, chạy unit test (pass), rồi tuyên bố "xong" — nhưng unit test vốn được thiết kế để cô lập (mock phụ thuộc) nên có "điểm mù" hệ thống với: không khớp giao diện giữa các component, lỗi lan truyền trạng thái (migration DB đổi schema nhưng cache ORM không hay biết), và phụ thuộc môi trường. Nghiêm trọng hơn: nghiên cứu 2026 của Anthropic cho thấy khi một agent tự đánh giá công việc của chính nó, nó luôn thiên vị tích cực — giải pháp không phải "làm agent khách quan hơn" mà là **tách người làm ra khỏi người kiểm** (generator vs. evaluator độc lập). Dữ liệu minh hoạ: kiến trúc một-agent chạy trần 20 phút/$9 cho ra game không chơi được; kiến trúc ba-agent (planner + generator + evaluator) chạy 6 giờ/$200 cho ra game chơi được hoàn toàn — cùng model, cùng prompt. Giải pháp thực tế: xác nhận kết thúc **ba lớp** (cú pháp/tĩnh → hành vi runtime → xác nhận cấp hệ thống end-to-end), và thông báo lỗi phải kèm hướng dẫn sửa cụ thể để agent tự điều chỉnh.

## Khái niệm chính

- **Tuyên bố hoàn thành sớm (Premature Completion Declaration)**: agent khẳng định xong khi đặc tả thật chưa được đáp ứng.
- **Sai lệch hiệu chuẩn độ tự tin (Confidence Calibration Bias)**: agent luôn tự tin hơn thực lực, đặc biệt với tác vụ đa tệp.
- **Tiêu chí kết thúc (Termination Criteria)**: điều kiện khách quan do harness quy định, thay cho phán đoán chủ quan.
- **Cổng kép xác minh - xác nhận (Verification-Validation Dual Gate)**: code đúng hành vi được chỉ định (verification) VÀ hệ thống đáp ứng yêu cầu end-to-end (validation).
- **Tách người làm khỏi người kiểm**: nguyên tắc quan trọng nhất của bài — evaluator độc lập, được huấn luyện để "kén chọn".

## Bài tập

1. Thiết kế quy trình xác nhận kết thúc ba lớp cho một tác vụ có migration DB + sửa API. Chạy trên tác vụ thật, ghi lại vấn đề ẩn nó phát hiện được.
2. Chọn 10 tác vụ, ghi độ tự tin tự báo cáo của agent và chất lượng thực tế. Tính sai lệch, phân tích quan hệ với độ phức tạp tác vụ.

## Luyện prompt

Viết đoạn "Định nghĩa hoàn thành" cho `CLAUDE.md` quy định rõ 3 cấp xác minh bắt buộc (unit → integration → end-to-end) và cấm chuyển cấp khi cấp trước fail — theo đúng mẫu trong bài giảng. Sau đó, đóng vai "evaluator": viết một prompt hệ thống ngắn cho một agent đánh giá độc lập, có nhiệm vụ "kén chọn", chỉ được duyệt khi có bằng chứng runtime cụ thể (không chấp nhận "trông có vẻ ổn").

Viết câu trả lời của bạn vào `bai-tap-prompt/answers/lecture-09.md` rồi nhờ Claude review.
