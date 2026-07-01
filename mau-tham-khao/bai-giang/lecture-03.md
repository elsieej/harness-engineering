# Bài mẫu tham khảo – Bài 03

> Ví dụ minh hoạ, không phải đáp án duy nhất. Đọc `../README.md` trước khi dùng file này.

## Bài tập 1 (mẫu) — Bài kiểm tra phiên mới

Mở phiên agent hoàn toàn mới trên LibraryAPI, chỉ cho xem repo, hỏi 5 câu:

| Câu hỏi | Trả lời được? |
|---|---|
| Hệ thống này là gì? | ✅ (nhờ AGENTS.md phần Tổng quan) |
| Tổ chức ra sao? | ⚠️ một phần — không biết vì sao `repositories/` tách khỏi `services/` |
| Chạy thế nào? | ✅ |
| Xác minh thế nào? | ✅ |
| Tiến độ hiện tại ra sao? | ❌ — không có PROGRESS.md |

Cải thiện: thêm `docs/architecture.md` giải thích lý do tách layer (để dễ mock DB khi test), thêm PROGRESS.md. Chạy lại bài kiểm tra: 5/5.

## Bài tập 2 (mẫu) — Khoảng cách hiển thị kiến thức

Liệt kê 8 quyết định/ràng buộc quan trọng của LibraryAPI, đánh dấu đã nằm trong repo chưa:

1. Mọi mutation mượn/trả PHẢI trong transaction — ❌ chưa (chỉ có trong đầu người viết service)
2. Phân trang `?page=&limit=` — ✅ (AGENTS.md)
3. Response shape `{data, meta}` — ✅
4. Không viết raw SQL ngoài repository — ✅
5. Số điện thoại độc giả dùng `libphonenumber-js` — ❌ chưa ghi ở đâu
6. Sách mượn quá 30 ngày tự động phạt — ❌ business rule chỉ có trong Jira, chưa vào repo
7. Test phải mock DB, không kết nối Postgres thật khi CI — ✅ (comment trong test file, chưa vào AGENTS.md)
8. Deploy qua GitHub Actions, cần secret `DATABASE_URL` — ✅ (README)

**Khoảng cách hiển thị kiến thức = 3/8 ≈ 37%.** Mục tiêu bài giảng: dưới 10% — cần viết ngay mục 1, 5, 6 vào repo trước khi coi là đạt.

## Luyện prompt (mẫu) — CONSTRAINTS.md

```markdown
## Ràng buộc giao dịch mượn/trả sách (ai cũng biết, chưa ai viết ra)

PHẢI: mọi thao tác ghi liên quan mượn/trả sách (tạo `loans` + cập nhật
`books.available_copies`) PHẢI được bọc trong một transaction Postgres,
dùng `withTransaction()` ở `src/db/transaction.ts`.

KHÔNG ĐƯỢC: gọi `booksRepository.update()` và `loansRepository.create()`
tách rời trong cùng một request handler — nếu tiến trình crash giữa hai
lệnh, số lượng sách khả dụng sẽ sai lệch vĩnh viễn và không tự phục hồi.

Lý do: đã từng xảy ra sai lệch tồn kho ở môi trường staging khi một
request bị timeout giữa hai lệnh ghi (không có transaction bảo vệ).
```

Đặt đoạn này ngay đầu `src/services/loanService.ts` — nơi rủi ro vi phạm cao nhất, không phải trong một tài liệu tổng quát ít ai mở. Dùng ngôn ngữ PHẢI/KHÔNG ĐƯỢC thay vì "nên", và có lý do cụ thể (sự cố thật) chứ không chỉ là quy tắc suông.
