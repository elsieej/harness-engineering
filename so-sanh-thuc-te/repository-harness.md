# So sánh thực tế: `hoangnb24/repository-harness` với kiến thức khoá học

Repo: [github.com/hoangnb24/repository-harness](https://github.com/hoangnb24/repository-harness) — CLI viết bằng Rust, 1000+ star, mục đích "biến bất kỳ repo nào thành không gian làm việc sẵn sàng cho agent" (Claude Code, Codex, Cursor...). Đây không phải bài tập của khoá học — là một công cụ thật, đang được dùng, đáng đối chiếu để xem nguyên lý ở `../bai-giang/` sống trong thực tế trông ra sao, và ở đâu người ta đi xa hơn hoặc bỏ sót so với khoá học.

Phân tích dựa trên đọc trực tiếp mã nguồn/tài liệu thật của repo (ADR, schema, docs), không phải suy đoán từ README.

## Những chỗ khớp hoặc vượt khoá học

| Bài giảng | Nhận định | Bằng chứng |
|---|---|---|
| [Bài 01](../bai-giang/lecture-01.md) — chẩn đoán harness trước khi đổi model | Mạnh hơn, có thể đo | `docs/HARNESS_MATURITY.md` có thang H0–H5 với tiêu chí đo được từng cấp, thay vì 5 lớp phân loại định tính như bài giảng |
| [Bài 02](../bai-giang/lecture-02.md) — 5 hệ thống phụ | Mạnh hơn, chi tiết hơn | `docs/HARNESS_COMPONENTS.md` dùng khung 11 mục (đặc tả tác vụ, chọn ngữ cảnh, quyền truy cập công cụ, bộ nhớ dự án, trạng thái tác vụ, quan sát, quy lỗi, xác minh, phân quyền, kiểm toán entropy, ghi nhận can thiệp), tự chấm Covered/Partial/Missing cho từng mục |
| [Bài 03](../bai-giang/lecture-03.md) — repo là nguồn sự thật | Tương đương, sắc hơn | ADR `0002`/`0003` hạ cấp đặc tả ban đầu xuống "tài liệu đầu vào, không phải sự thật sống lâu dài"; `docs/CONTEXT_RULES.md` có bảng Phải/Nên/Bỏ qua theo giai đoạn×mức rủi ro, thay thế "bài kiểm tra phiên mới" |
| [Bài 04](../bai-giang/lecture-04.md) — chống phình hướng dẫn | Triệt để hơn | `AGENTS.md` gốc chỉ ~15 dòng, thuần định tuyến, 0 ràng buộc nhúng trực tiếp — mỏng hơn cả mục tiêu 100 dòng của khoá học |
| [Bài 11](../bai-giang/lecture-11.md) — quan sát runtime | Mạnh hơn | `docs/TRACE_SPEC.md` cho điểm chất lượng trace tự động (`score-trace`, `score-context`, `audit`) — định lượng hơn hẳn cách mô tả định tính của bài giảng |

## Những chỗ yếu hơn, chiếu theo đúng rubric của khoá học

1. **Vi phạm đúng điều quan trọng nhất của [Bài 08](../bai-giang/lecture-08.md):** agent tự báo cờ hoàn thành (`story update --unit 1 --integration 1 ...`). Có lệnh `story verify` chạy xác minh thật, nhưng theo `US-017`, bước kiểm tra trước khi đóng story **chỉ mang tính khuyến cáo — chỉ cảnh báo, không chặn**. Cổng có tồn tại nhưng không thực sự "chặn".
2. **Thiếu cơ chế cốt lõi của [Bài 09](../bai-giang/lecture-09.md):** không có vai trò evaluator độc lập. Cùng một agent viết code, tự báo cáo, tự ghi trace của chính nó. Có bảng `intervention` để ghi lại việc con người can thiệp sau đó, nhưng không có gì bắt buộc một vòng đánh giá độc lập trước khi đóng việc.
3. **Không có tương đương với WIP=1 ở [Bài 07](../bai-giang/lecture-07.md)** — các "làn rủi ro" (tiny/normal/high-risk) giới hạn phạm vi ảnh hưởng, không giới hạn số việc đang làm song song.
4. **Ràng buộc kiến trúc ở [Bài 10](../bai-giang/lecture-10.md) chưa được tự động hoá**: `docs/ARCHITECTURE.md` có bảng quy tắc phụ thuộc rất tốt, nhưng tài liệu tự gọi mình là "khuôn mẫu tư duy, không phải giàn kiểm tra" — không có script lint nào thực thi các quy tắc đó.
5. **Không có checklist thoát phiên / script dọn dẹp idempotent như [Bài 12](../bai-giang/lecture-12.md)**, và đáng chú ý: toàn bộ trạng thái liên tục nằm trong SQLite `harness.db` lại bị `.gitignore` — đi ngược nguyên tắc "trạng thái quan trọng phải được ghi vào tệp git-tracked" của [Bài 03](../bai-giang/lecture-03.md)/[Bài 05](../bai-giang/lecture-05.md). Clone mới sẽ mất sạch lịch sử liên tục.

## Ý tưởng mới, khoá học chưa từng đề cập

- **Lớp lưu trữ bền vững bằng SQLite** có schema truy vấn được, thay vì các tệp markdown như `PROGRESS.md`.
- **Thang trưởng thành harness H0–H5** — một mô hình capability-maturity hình thức hoá, khoá học không có khái niệm tương đương.
- **Pipeline tự cải thiện** (`harness-cli propose`, ADR `0007`) — biến friction/can thiệp/độ trôi dạt lặp lại thành đề xuất backlog có bằng chứng, chờ con người duyệt.
- **Registry công cụ** với cơ chế dò sự hiện diện (`tool check`) cho năng lực bên ngoài (cli/binary/mcp/skill/http).
- **Điểm entropy/độ trôi dạt định lượng** — một phiên bản có số hoá của ý tưởng "thử ablation" ở Bài 02/12.

## Kết luận

`repository-harness` là **một "người anh em họ" nghiêm túc, tham vọng hơn về hạ tầng, chứ không phải một bản triển khai của khoá học này** — được xây dựng độc lập (ADR trích dẫn báo khoa học, không trích khoá học này) và khác triết lý: nó tối ưu cho **công cụ + đo lường** (database bền vững, thang trưởng thành, vòng lặp tự đề xuất cải thiện, registry công cụ) thay vì **kỷ luật theo từng phiên làm việc** (giới hạn WIP, bắt buộc đánh giá độc lập, checklist thoát phiên) mà khoá học coi là không thể thương lượng.

Hai triết lý này **bổ sung cho nhau, không loại trừ nhau**: một đội có thể áp dụng ý tưởng lớp bền vững + thang trưởng thành của repo này, đặt bên trên các quy tắc chặt hơn của khoá học (cổng trạng thái chỉ do script xác minh, tách vai trò generator/evaluator, WIP=1), và có được một hệ thống mạnh hơn cả hai nguồn cộng lại.
