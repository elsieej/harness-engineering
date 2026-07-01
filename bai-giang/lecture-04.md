# Bài 04. Chia hướng dẫn ra thành nhiều tệp

Bản đầy đủ: [../../learn-harness-engineering/docs/vi/lectures/lecture-04-why-one-giant-instruction-file-fails/index.md](../../learn-harness-engineering/docs/vi/lectures/lecture-04-why-one-giant-instruction-file-fails/index.md)

## Tóm tắt

Bẫy phổ biến: mỗi lần agent mắc lỗi, bạn "thêm một quy tắc" vào `AGENTS.md`. Vài tháng sau tệp phình lên 600 dòng, và hiệu suất agent bắt đầu tệ đi vì năm lý do: ngân sách ngữ cảnh bị ăn mòn (10-20K token chỉ để đọc hướng dẫn); hiệu ứng "lạc giữa chừng" (Lost in the Middle — Liu et al. 2023) khiến ràng buộc quan trọng chôn ở dòng 300 dễ bị bỏ qua; xung đột ưu tiên vì ràng buộc cứng và gợi ý mềm trông giống hệt nhau; khó bảo trì (chỉ tăng không giảm); và tích luỹ mâu thuẫn. Giải pháp: giữ tệp đầu vào ngắn (50-200 dòng, chỉ tổng quan + lệnh chạy + ràng buộc cứng + liên kết), tách chi tiết sang các tài liệu chủ đề trong `docs/` hoặc cạnh module, tải "theo yêu cầu" (reveal on demand) — giống sắp đồ trong vali theo từng túi thay vì đổ hết ra. Một nhóm SaaS thực hiện tái cấu trúc kiểu này: tỷ lệ thành công tăng từ 45% lên 72%, tuân thủ ràng buộc bảo mật tăng từ 60% lên 95%.

## Khái niệm chính

- **Phình hướng dẫn (Instruction Bloat)**: tệp hướng dẫn chiếm 10-15%+ cửa sổ ngữ cảnh.
- **Lạc giữa chừng (Lost in the Middle)**: LLM tận dụng thông tin ở giữa văn bản dài kém hơn ở đầu/cuối.
- **Tỷ lệ tín hiệu trên nhiễu (Instruction SNR)**: tỷ lệ hướng dẫn thực sự liên quan tới tác vụ đang làm.
- **Tệp đầu vào (Entry File)**: bộ định tuyến ngắn, không phải bách khoa toàn thư.
- **Tiết lộ theo yêu cầu (Reveal on Demand)**: tổng quan trước, chi tiết khi cần — giống progressive disclosure trong UI.

## Bài tập

1. Kiểm toán SNR: liệt kê mọi mục trong AGENTS.md hiện tại, chọn 5 loại tác vụ thường gặp, đánh dấu mục nào liên quan tới tác vụ nào. Chuyển các mục là "nhiễu" với đa số tác vụ sang tài liệu chủ đề.
2. Nếu tệp hướng dẫn của bạn trên 300 dòng: chia thành tệp đầu vào <100 dòng + 3-5 tài liệu chủ đề. So sánh tỷ lệ thành công trước/sau trên ít nhất 5 tác vụ.

## Luyện prompt

Giả sử bạn có một `AGENTS.md` tưởng tượng đã phình tới 400 dòng (trộn lẫn: quy ước code, ghi chú sửa lỗi lịch sử, hướng dẫn triển khai, sở thích cá nhân). Viết kế hoạch tái cấu trúc cụ thể: (a) nội dung nào giữ lại trong tệp đầu vào rút gọn (đưa ra danh sách các mục, không quá 15 ràng buộc cứng), (b) bạn sẽ tách thành những tài liệu chủ đề nào (đặt tên file, ước lượng độ dài), (c) nội dung nào nên xoá hẳn hoặc chuyển thành test case. Trình bày như một prompt/kế hoạch bạn sẽ giao cho agent thực hiện việc tái cấu trúc đó.

Viết câu trả lời của bạn vào `bai-tap-prompt/answers/lecture-04.md` rồi nhờ Claude review.
