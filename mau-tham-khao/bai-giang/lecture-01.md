# Bài mẫu tham khảo – Bài 01

> Đây là ví dụ minh hoạ, không phải đáp án duy nhất. Đọc `../README.md` trước khi dùng file này.

## Bài tập 1 (mẫu)

Dự án: **LibraryAPI**. Tác vụ: "Thêm endpoint tìm sách theo tên tác giả, có phân trang."

**Chạy agent trần** (chỉ giao đúng câu trên, không AGENTS.md, không lệnh xác minh): sau 20 phút, agent tạo `GET /books/search?author=`, nhưng không phân trang và không viết test. Test thủ công với 500+ record giả lập: response trả toàn bộ, không giới hạn — sẽ sập trong thực tế.

Quy mỗi lỗi vào 5 lớp:

| Lớp | Biểu hiện cụ thể |
|---|---|
| Đặc tả | "Phân trang" không được định nghĩa: query param nào, page size mặc định bao nhiêu |
| Ngữ cảnh | Agent không biết `/members` đã có convention `?page=&limit=` sẵn, nên tự bịa cách khác |
| Môi trường | Không có sẵn seed data lớn để bộc lộ vấn đề khi test |
| Xác minh | Không có lệnh nào được yêu cầu chạy trước khi báo "xong" |
| Trạng thái | Không áp dụng — tác vụ 1 phiên |

**Thêm AGENTS.md** (nêu convention phân trang có sẵn + bắt buộc `npm test`/`npm run lint` trước khi báo xong), chạy lại: agent tự tìm và tái dùng convention của `/members`, viết kèm test phân trang, `npm test` pass thật.

## Bài tập 2 (mẫu)

5 tác vụ nhỏ trên LibraryAPI: (1) thêm field `publishedYear`, (2) sửa validation email độc giả, (3) thêm index DB cho tra cứu ISBN, (4) thêm xoá mềm sách, (5) sửa lỗi định dạng ngày trả sách. Agent tự báo "xong" cả 5/5.

Xác minh độc lập bằng test: 3/5 pass thật; 2/5 fail khi kiểm tra kỹ — (2) thiếu case email rỗng, (4) quên viết migration nên cột `deleted_at` không tồn tại ở DB thật dù code đã dùng.

**Khoảng cách xác minh = 2/5 = 40%.** Cả hai ca fail đều thuộc loại "trông ổn nhưng chưa chạy hết pipeline" — chủ đề sẽ khai triển kỹ ở bài 09 và 10.

## Luyện prompt (mẫu) — Definition of Done

> Giao cho agent nhiệm vụ "thêm tính năng tìm kiếm" vào LibraryAPI. Định nghĩa Hoàn thành:

**Endpoint:** `GET /books/search?author=<string>&page=<int>&limit=<int>`

**Hành vi mong đợi:**
- Tìm không phân biệt hoa/thường, khớp một phần tên tác giả (`ILIKE '%...%'`)
- Phân trang theo convention có sẵn của `/members` (`page` mặc định 1, `limit` mặc định 20, tối đa 100)
- Response: `{ data: Book[], meta: { total, page, limit } }`
- Không tìm thấy → `data: []`, KHÔNG trả lỗi 404
- `author` rỗng hoặc thiếu → lỗi 400 dạng `{ error: { code: "INVALID_QUERY", message: "..." } }`

**Lệnh xác minh bắt buộc (phải pass cả 3 trước khi báo xong):**
```bash
npm run lint
npm test -- tests/routes/books.search.test.ts
npm test   # toàn bộ suite, đảm bảo không phá vỡ endpoint khác
```

Đây là loại DoD giúp agent không phải đoán — mọi câu trong "hành vi mong đợi" đều kiểm chứng được bằng một dòng lệnh, không có chỗ nào cần diễn giải chủ quan.
