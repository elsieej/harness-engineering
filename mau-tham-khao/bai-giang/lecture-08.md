# Bài mẫu tham khảo – Bài 08

> Ví dụ minh hoạ, không phải đáp án duy nhất. Đọc `../README.md` trước khi dùng file này.

## Bài tập 1 (mẫu) — Schema feature list tối giản

```json
{
  "id": "string, ví dụ 'fine-02'",
  "hanh_vi": "string, mô tả hành vi có thể đúng/sai rõ ràng",
  "lenh_xac_minh": "string, một lệnh shell chạy được, trả exit code 0 khi pass",
  "trang_thai": "enum: not_started | active | blocked | passing",
  "bang_chung": "string, output/log của lần chạy lệnh xác minh gần nhất"
}
```
Áp dụng cho 5 tính năng LibraryAPI (tìm sách, phân trang loans, phạt trễ hạn, xoá mềm sách, đổi mật khẩu độc giả) — mỗi mục đều điền đủ 5 trường trước khi coi là sẵn sàng giao cho agent.

## Bài tập 2 (mẫu) — Xác minh lỏng vs. nghiêm

Với 3 tính năng (tìm sách, phạt trễ hạn, đổi mật khẩu):
- Xác minh **lỏng**: "chạy `npm test`, nếu không có lỗi đỏ thì pass" → cả 3/3 "pass" (dương tính giả cao vì test coverage thấp).
- Xác minh **nghiêm**: mỗi tính năng có lệnh xác minh end-to-end cụ thể (`curl` thật + kiểm tra giá trị trả về chính xác) → chỉ 2/3 pass thật; tính năng đổi mật khẩu lộ lỗi không hash lại token phiên cũ.

Tỷ lệ dương tính giả giữa 2 cách: lỏng 3/3 "pass" nhưng nghiêm chỉ 2/3 — chênh lệch 33% chính là khoảng cách xác minh bị che giấu bởi tiêu chí lỏng.

## Luyện prompt (mẫu) — Hiệu chỉnh độ hạt + quy tắc gating

**Quá rộng** → "Triển khai toàn bộ hệ thống phạt trễ hạn" viết lại thành các mục đúng độ hạt (xem bảng bài 07: fine-01 → fine-05, mỗi mục hoàn thành trong một phiên).

**Quá hẹp** → "Tạo trường `isbn` trên model Book" (quá vụn, không đáng một mục riêng) gộp lại thành mục có ý nghĩa:
```json
{
  "id": "book-isbn-01",
  "hanh_vi": "Thêm cột isbn (unique, not null) vào books, validate format ISBN-13 khi tạo/sửa sách qua API",
  "lenh_xac_minh": "npm run db:migrate && npm test tests/routes/books.isbn.test.ts",
  "trang_thai": "not_started"
}
```
(gộp cả migration + validation + test vào một mục "hoàn thành được trong một phiên", thay vì tách "tạo trường" và "thêm validate" thành 2 mục vụn không có giá trị độc lập).

**Quy tắc gating** (thêm vào `CLAUDE.md`):
```markdown
KHÔNG ĐƯỢC tự sửa trường `trang_thai` trong feature list. Trạng thái
chỉ được chuyển sang `passing` bởi script `scripts/verify.sh <id>` —
script này chạy đúng `lenh_xac_minh` của mục đó, và chỉ ghi `passing`
kèm `bang_chung` (output thật) khi lệnh trả exit code 0.
```
