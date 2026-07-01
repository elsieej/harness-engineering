# Bài mẫu tham khảo – Bài 02

> Ví dụ minh hoạ, không phải đáp án duy nhất. Đọc `../README.md` trước khi dùng file này.

## Bài tập 1 (mẫu) — Kiểm toán 5 hệ thống phụ

Dự án LibraryAPI hiện tại, chấm 1-5:

| Hệ thống phụ | Điểm | Ghi chú |
|---|---|---|
| Hướng dẫn | 2 | Chỉ có README cài đặt, không có ràng buộc/convention nào viết ra |
| Công cụ | 4 | Có sẵn Jest, ESLint, npm scripts đầy đủ |
| Môi trường | 3 | Có docker-compose cho Postgres nhưng không pin version |
| Trạng thái | 1 | Không có PROGRESS.md, chỉ dựa vào git log |
| Phản hồi | 3 | Có test nhưng không bắt buộc chạy trước khi merge |

Điểm thấp nhất: **Trạng thái (1/5)**. Cải thiện trong 30 phút: thêm `PROGRESS.md` với 4 mục (trạng thái, đã xong, đang làm, bước tiếp theo). Quan sát: phiên agent tiếp theo tự tiếp tục đúng việc đang dở, không hỏi lại "hiện tại project ở đâu rồi".

## Bài tập 2 (mẫu) — Loại trừ biến có kiểm soát

Tác vụ: "thêm validate số điện thoại độc giả". Gỡ lần lượt:
- Gỡ **hướng dẫn** (không AGENTS.md): agent tự bịa format số điện thoại, không khớp convention có sẵn.
- Gỡ **phản hồi** (không yêu cầu chạy test): agent báo xong nhưng regex validate sai với số có mã vùng.
- Gỡ **trạng thái** (không PROGRESS.md, tác vụ chia 2 phiên): phiên 2 làm lại từ đầu vì không biết phiên 1 đã quyết định dùng thư viện `libphonenumber-js`.

Xếp hạng giá trị biên (tác vụ này): Trạng thái > Hướng dẫn > Phản hồi — vì tác vụ chia nhiều phiên nên mất ngữ cảnh gây thiệt hại lớn nhất.

## Luyện prompt (mẫu) — AGENTS.md

```markdown
# AGENTS.md — LibraryAPI

## Tổng quan
REST API thư viện sách nội bộ. Node.js 20 + Express + PostgreSQL 15 + Jest.
Ba miền: sách (/books), độc giả (/members), phiếu mượn (/loans).

## Lệnh khởi động
    npm install
    docker-compose up -d      # Postgres cục bộ
    npm run db:migrate
    npm run dev                # server tại :3000
    npm test
    npm run lint

## Ràng buộc cứng
1. PHẢI dùng ?page=&limit= cho mọi endpoint danh sách (mặc định limit=20, tối đa 100).
2. PHẢI trả { data, meta }; lỗi dạng { error: { code, message } } — xem src/utils/response.ts.
3. KHÔNG ĐƯỢC viết raw SQL ngoài src/repositories/* — mọi query qua repository layer.
4. PHẢI có migration kèm mọi thay đổi schema, KHÔNG sửa tay bảng qua psql.
5. PHẢI viết test cho route mới trong tests/routes/*.test.ts trước khi báo hoàn thành.
6. KHÔNG ĐƯỢC merge khi `npm test` hoặc `npm run lint` fail.

## Trạng thái
Xem PROGRESS.md trước khi bắt đầu; cập nhật lại trước khi kết thúc phiên.

## Xác minh
`npm test && npm run lint` phải pass trước khi coi bất kỳ việc gì là xong.

## Tài liệu chi tiết (đọc khi cần)
- docs/architecture.md — luồng dữ liệu, layer
- docs/db-schema.md — schema đầy đủ + lý do các quyết định index
```

44 dòng — đủ 5 hệ thống phụ, dưới 100 dòng, ràng buộc cứng ở dạng thực thi được (không liệt kê chi tiết implementation).
