# Chỉ mục 12 bài luyện prompt

Mỗi bài giảng trong `../bai-giang/` có một phần "Luyện prompt" — một tình huống thực tế buộc bạn viết một prompt, hoặc một artifact harness (AGENTS.md, feature_list.json, sprint contract, v.v.) nhắm vào một hệ model+harness cụ thể. Danh sách dưới đây chỉ tóm tắt kịch bản; mở file lecture tương ứng để xem đầy đủ yêu cầu.

| # | Kịch bản (tóm tắt) | Chi tiết |
|---|---|---|
| 01 | Viết Định nghĩa Hoàn thành cho tính năng tìm kiếm | [lecture-01.md](../bai-giang/lecture-01.md) |
| 02 | Viết một `AGENTS.md` đủ 5 hệ thống phụ, dưới 100 dòng | [lecture-02.md](../bai-giang/lecture-02.md) |
| 03 | Viết `ARCHITECTURE.md`/`CONSTRAINTS.md` cho một ràng buộc ngầm | [lecture-03.md](../bai-giang/lecture-03.md) |
| 04 | Lên kế hoạch tái cấu trúc một AGENTS.md 400 dòng đã phình to | [lecture-04.md](../bai-giang/lecture-04.md) |
| 05 | Viết `PROGRESS.md` + `DECISIONS.md` cho một tính năng đang dở | [lecture-05.md](../bai-giang/lecture-05.md) |
| 06 | Viết prompt "chỉ khởi tạo" (cấm code tính năng) cho phiên đầu | [lecture-06.md](../bai-giang/lecture-06.md) |
| 07 | Viết quy tắc WIP=1 + feature-list 3 trường thực thi được | [lecture-07.md](../bai-giang/lecture-07.md) |
| 08 | Viết lại tính năng "quá rộng"/"quá hẹp" về đúng độ hạt | [lecture-08.md](../bai-giang/lecture-08.md) |
| 09 | Viết Định nghĩa Hoàn thành 3 cấp + prompt evaluator "kén chọn" | [lecture-09.md](../bai-giang/lecture-09.md) |
| 10 | Viết quy tắc kiến trúc PHẢI/KHÔNG ĐƯỢC + kiểm tra tự động | [lecture-10.md](../bai-giang/lecture-10.md) |
| 11 | Viết sprint contract + rubric chấm điểm 3-4 chiều | [lecture-11.md](../bai-giang/lecture-11.md) |
| 12 | Viết session exit checklist + script dọn dẹp idempotent | [lecture-12.md](../bai-giang/lecture-12.md) |

## Cách làm

1. Đọc phần "Luyện prompt" trong file lecture tương ứng.
2. Viết câu trả lời của bạn vào `answers/lecture-XX.md` (thay XX bằng số bài, 2 chữ số).
3. Nhắn cho Claude trong phiên chat, ví dụ: "review bài luyện prompt lecture-03 của tôi" — Claude sẽ đối chiếu với nguyên tắc harness engineering đã học ở bài đó và góp ý cụ thể (thiếu artifact nào, có mơ hồ chỗ nào, lệnh xác minh có thực thi được không, v.v.).
4. Sửa lại theo góp ý, đánh dấu hoàn thành trong bảng ở `../00-LO-TRINH.md`.

Không có gì bắt buộc phải làm tuần tự — nhưng vì mỗi bài xây trên khái niệm bài trước, làm theo thứ tự 01 → 12 sẽ dễ tiếp thu hơn.
