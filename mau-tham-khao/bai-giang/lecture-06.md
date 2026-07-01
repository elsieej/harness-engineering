# Bài mẫu tham khảo – Bài 06

> Ví dụ minh hoạ, không phải đáp án duy nhất. Đọc `../README.md` trước khi dùng file này.

## Bài tập 1 (mẫu)

Danh sách sẵn sàng cho LibraryAPI, đưa cho phiên agent hoàn toàn mới. Vấn đề gặp phải: agent chạy được `npm install` nhưng `npm run dev` lỗi vì thiếu `.env` mẫu — đây là điều khoản còn thiếu. Bổ sung `.env.example` + dòng hướng dẫn copy vào checklist. Chạy lại: agent khởi động, chạy test, đọc PROGRESS.md hiểu tiến độ, tự chọn được bước tiếp theo — 0 vấn đề còn lại.

## Bài tập 2 (mẫu)

Dự án mới "NotifyService" (giả định, gửi thông báo mượn/trả quá hạn). So sánh 4 phiên:

| | (A) Vừa khởi tạo vừa làm tính năng đầu | (B) 1 phiên riêng cho khởi tạo |
|---|---|---|
| Thời gian tới test pass đầu tiên | Phiên 2, ~50 phút | Phiên 1, ~20 phút |
| Chi phí tái thiết lập phiên 3 | ~15 phút (giả định môi trường thiếu vài bước) | ~3 phút |
| Tỷ lệ hoàn thành sau 4 phiên | 2/3 tính năng dự kiến | 3/3 tính năng dự kiến |

Kết quả khớp với dữ liệu bài giảng (tăng ~31% tỷ lệ hoàn thành nhờ tách giai đoạn khởi tạo) — dù đây chỉ là số liệu minh hoạ, không phải đo thật.

## Luyện prompt (mẫu) — Prompt "chỉ khởi tạo"

```
Đây là phiên đầu tiên của dự án NotifyService (REST API nhỏ gửi thông
báo mượn/trả quá hạn qua email). NHIỆM VỤ DUY NHẤT của phiên này là
khởi tạo hạ tầng — TUYỆT ĐỐI KHÔNG được viết bất kỳ logic nghiệp vụ
nào (không viết endpoint gửi thông báo, không viết template email).

Trước khi kết thúc phiên, PHẢI tạo đủ 4 thứ:
1. Môi trường chạy được: `npm install && npm run dev` khởi động server
   rỗng (chỉ có route health-check `/health`) không lỗi.
2. Ít nhất 1 test mẫu pass: test cho `/health` trả 200.
3. Tài liệu "danh sách sẵn sàng" (STARTUP.md): liệt kê mọi bước để một
   agent mới chạy được dự án từ đầu.
4. Bản phân rã tác vụ ≥3 mục, mỗi mục có tiêu chí chấp nhận cụ thể, ví dụ:
   - "Endpoint POST /notifications/loan-overdue" — chấp nhận khi: gọi
     endpoint với loan_id hợp lệ trả 202, test tích hợp pass.
   - "Kết nối SMTP thật (hoặc mock trong test)" — chấp nhận khi: test
     gửi email giả lập pass, không gọi SMTP thật trong CI.
   - "Cấu hình retry khi gửi thất bại" — chấp nhận khi: test mô phỏng
     lỗi mạng, xác nhận có 3 lần retry với backoff.

Checklist chấp nhận phiên khởi tạo (dùng để xác nhận đã đạt):
- [ ] `npm run dev` chạy không lỗi từ repo sạch (clone mới)
- [ ] `npm test` pass với ≥1 test
- [ ] STARTUP.md tồn tại và đủ để agent khác làm theo không cần hỏi
- [ ] Có ≥3 mục phân rã, mỗi mục có tiêu chí chấp nhận rõ ràng
- [ ] Có 1 git commit checkpoint sạch
```
