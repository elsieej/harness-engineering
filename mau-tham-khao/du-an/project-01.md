# Ví dụ minh hoạ – Dự án 01. Chỉ Prompt vs. Ưu tiên Quy tắc

> ⚠️ Số liệu dưới đây là **minh hoạ giả định**, không phải kết quả đo thật (Claude chưa tự chạy ứng dụng Electron này). Tham khảo hình dạng, không phải con số tuyệt đối. Khi bạn tự chạy, hãy thay bằng số thật của bạn.

## Thiết lập
- [x] Copy `starter/` thành 2 bản: `attempt-a-baseline/` và `attempt-b-harness/`
- [x] Bản B thêm `AGENTS.md` (5 hệ thống phụ), `init.sh`, `feature_list.json`, `claude-progress.md`

## Nhánh A — Baseline (chỉ prompt)
- Thời gian: ~20 phút
- Số vòng lặp thử-sai: 1 (agent chạy 1 lèo, không tự kiểm tra lại)
- Tỷ lệ tính năng pass xác minh end-to-end: 1/4 tính năng cốt lõi (danh sách tài liệu) chạy được, 3/4 còn lại (import, Q&A, panel trạng thái) lỗi hoặc thiếu
- Quan sát: agent tự báo "hoàn thành ứng dụng knowledge-base" dù UI trống rỗng phần lớn — đúng mô tả "capability gap" ở bài 01

## Nhánh B — Có harness
- Thời gian: ~2.5 giờ (dài hơn baseline đáng kể — đúng đánh đổi mà bài giảng gốc mô tả)
- Số vòng lặp thử-sai: 4 (mỗi vòng đối chiếu `feature_list.json`, sửa xong mới qua mục tiếp theo — WIP=1)
- Tỷ lệ tính năng pass xác minh end-to-end: 4/4
- Quan sát: agent tự dừng đúng lúc mỗi tính năng "trông ổn" để chạy lệnh xác minh trong feature list, không tự tuyên bố xong sớm

## So sánh & nhận xét
Khác biệt lớn nhất không phải "agent viết code tốt hơn" mà là **agent biết dừng đúng lúc để xác minh** — đúng luận điểm cốt lõi bài 01-02: cùng model, harness khác nhau ra kết quả khác hẳn. Artifact đóng góp nhiều nhất (ước lượng chủ quan): `feature_list.json`, vì nó là thứ duy nhất ngăn agent tự nhận "xong".

## Đối chiếu với `solution/`
_(làm sau khi tự chạy — ghi lại điểm bản của bạn khác `solution/` ở đâu, không sao chép trước)_
