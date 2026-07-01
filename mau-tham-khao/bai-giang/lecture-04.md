# Bài mẫu tham khảo – Bài 04

> Ví dụ minh hoạ, không phải đáp án duy nhất. Đọc `../README.md` trước khi dùng file này.

## Bài tập 1 (mẫu) — Kiểm toán SNR

AGENTS.md của LibraryAPI (giả định đã phình lên 220 dòng qua nhiều tháng vá lỗi). 5 loại tác vụ thường gặp: thêm endpoint mới / sửa bug validation / thêm migration DB / viết test / deploy. Đánh dấu từng mục có liên quan tác vụ nào:

| Mục trong AGENTS.md | Liên quan mấy/5 loại tác vụ |
|---|---|
| Convention phân trang, response shape | 5/5 — giữ lại tệp đầu vào |
| Chi tiết cấu hình CI/CD từng bước | 1/5 (chỉ deploy) — chuyển sang docs/deployment.md |
| Lịch sử 12 lần sửa lỗi ISBN trùng | 0/5 — nhiễu thuần tuý, nên xoá hoặc chuyển thành test case |
| Quy ước đặt tên biến cá nhân của dev cũ | 0/5 — xoá, không còn giá trị thực thi |
| Cách viết migration | 3/5 — chuyển sang docs/db-conventions.md, để lại 1 dòng dẫn link |

Kết luận: >60% nội dung hiện tại là nhiễu với đa số tác vụ — cần tái cấu trúc.

## Bài tập 2 (mẫu)

AGENTS.md hiện 220 dòng (>300 chưa tới nhưng đã bắt đầu ảnh hưởng). Chia thành tệp đầu vào ~45 dòng (như mẫu bài 02) + 3 tài liệu chủ đề. So sánh 5 tác vụ trước/sau: tỷ lệ agent tự tìm đúng convention phân trang mà không hỏi lại tăng từ 2/5 lên 5/5 — vì trước đây convention đó bị chôn ở dòng 180.

## Luyện prompt (mẫu) — Kế hoạch tái cấu trúc

**AGENTS.md 400 dòng giả định** (trộn: quy ước code, lịch sử sửa lỗi, hướng dẫn triển khai, sở thích cá nhân).

**(a) Giữ lại trong tệp đầu vào rút gọn** (≤15 ràng buộc cứng): tổng quan dự án, lệnh khởi động, 6 ràng buộc PHẢI/KHÔNG ĐƯỢC cốt lõi (transaction, response shape, phân trang, repository layer, migration, test bắt buộc), link tới các tài liệu chủ đề bên dưới.

**(b) Tách thành tài liệu chủ đề:**
- `docs/architecture.md` (~80 dòng) — sơ đồ layer, lý do tách repository/service
- `docs/db-conventions.md` (~60 dòng) — quy tắc migration, naming column, index
- `docs/testing.md` (~50 dòng) — cách mock DB, cấu trúc thư mục test
- `docs/deployment.md` (~40 dòng) — CI/CD, secrets cần thiết

**(c) Xoá hẳn hoặc chuyển thành test:**
- Lịch sử "đã sửa lỗi X vào tháng Y" → nếu lỗi có thể tái diễn, viết thành regression test trong `tests/regressions/`; nếu không, xoá.
- Sở thích cá nhân không thực thi được ("nên đặt tên biến rõ ràng") → xoá, không kiểm chứng được nên vô giá trị với agent.

Trình bày lại thành prompt giao cho agent: *"Tái cấu trúc AGENTS.md theo kế hoạch (a)(b)(c) ở trên. Sau khi xong, chạy bài kiểm tra phiên mới (5 câu hỏi ở bài 03) để xác nhận không mất thông tin quan trọng."*
