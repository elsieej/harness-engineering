# Bài mẫu tham khảo – Bài 05

> Ví dụ minh hoạ, không phải đáp án duy nhất. Đọc `../README.md` trước khi dùng file này.

## Bài tập 1 (mẫu)

Tác vụ 3 phiên: "thêm phân trang cho endpoint `/loans` + tối ưu query kèm theo". Không dùng PROGRESS.md: phiên 2 mất ~18 phút đọc lại code + git log để hiểu phiên 1 đã làm gì và tại sao chọn cách đó; phiên 3 mất ~22 phút vì phiên 2 đổi hướng mà không ghi lại lý do. Thêm PROGRESS.md + DECISIONS.md, lặp lại tác vụ tương tự: phiên 2 và 3 chỉ mất **dưới 3 phút** để vào trạng thái làm việc — đọc PROGRESS.md là đủ.

## Bài tập 2 (mẫu) — Mẫu bàn giao tối giản

```markdown
## Bàn giao phiên
- Trạng thái repo: commit abc123f, `npm test` — 42/42 pass
- Trạng thái runtime: server chạy được ở :3000, migration mới nhất đã áp dụng
- Chướng ngại: query đếm tổng số loan chậm khi >10k record (chưa có index)
- Hành động kế tiếp: thêm index trên `loans.member_id`, đo lại thời gian query
```

Thử nghiệm: phiên hoàn toàn mới chỉ đọc 4 dòng này, khôi phục đúng trạng thái và biết chính xác việc tiếp theo trong ~90 giây — đạt mục tiêu "dưới 3 phút".

## Luyện prompt (mẫu) — PROGRESS.md + DECISIONS.md

**PROGRESS.md** (tính năng: "thêm phân trang cho endpoint `/loans`"):
```markdown
## Trạng thái hiện tại
Commit: def456a. `npm test`: 38/40 pass (2 test phân trang mới đang viết dở).

## Đã hoàn thành
- Thêm query param page/limit vào route handler
- Cập nhật repository để LIMIT/OFFSET đúng

## Đang thực hiện
- Viết test cho case limit vượt quá 100 (phải clamp về 100)

## Vấn đề đã biết
- Query COUNT(*) tổng số loan chưa tối ưu, sẽ chậm với dataset lớn — chưa xử lý trong tác vụ này

## Bước tiếp theo
1. Hoàn thành test case clamp limit
2. Chạy full suite, xác nhận 40/40
3. Cân nhắc mở tác vụ riêng cho tối ưu COUNT(*)
```

**DECISIONS.md:**
```markdown
## Quyết định: dùng LIMIT/OFFSET thay vì cursor-based pagination

Ngày: (điền ngày thật)
Chọn: LIMIT/OFFSET theo convention có sẵn ở `/members`.
Lý do: nhất quán với endpoint khác, dữ liệu `loans` không đủ lớn để
cursor-based mang lại lợi ích rõ rệt ở giai đoạn này.
Phương án bị loại: cursor-based (keyset pagination) — hiệu năng tốt hơn
ở dataset rất lớn, nhưng phức tạp hơn và không nhất quán với phần còn
lại của API. Sẽ cân nhắc lại nếu `loans` vượt 1 triệu record.
```

Một phiên mới đọc 2 file này là biết chính xác "đang ở đâu, vì sao chọn cách này, làm gì tiếp" — không cần hỏi lại.
