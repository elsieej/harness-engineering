# Bài mẫu tham khảo – Bài 09

> Ví dụ minh hoạ, không phải đáp án duy nhất. Đọc `../README.md` trước khi dùng file này.

## Bài tập 1 (mẫu) — Xác nhận kết thúc 3 lớp

Tác vụ: migration đổi `books.available_copies` thành tính toán (thay vì lưu trực tiếp) + sửa API `/books/:id`.

- **Lớp 1 (cú pháp/tĩnh):** `npm run lint && tsc --noEmit` — pass.
- **Lớp 2 (hành vi runtime):** `npm test` — unit test pass, nhưng...
- **Lớp 3 (end-to-end):** chạy thật `docker-compose up` + gọi `/loans` tạo phiếu mượn mới → phát hiện `books.available_copies` trả `undefined` vì endpoint `/books/:id` cũ chưa được cập nhật để đọc từ cách tính mới. Unit test pass vì nó mock repository, không phát hiện được lỗi tích hợp giữa 2 endpoint.

Vấn đề ẩn chỉ lộ ra ở lớp 3 — đúng bài học cốt lõi: unit test có điểm mù hệ thống với lỗi liên endpoint.

## Bài tập 2 (mẫu)

10 tác vụ trên LibraryAPI, agent tự báo độ tự tin (thang 1-10) và chất lượng thực tế đo bằng test độc lập:

| Độ phức tạp tác vụ | Độ tự tin trung bình agent báo | Chất lượng thực tế (tỷ lệ pass) |
|---|---|---|
| Đơn giản (1 file, 1 endpoint) | 9/10 | 90% khớp |
| Trung bình (2-3 file, có migration) | 8.5/10 | 65% khớp |
| Phức tạp (đa endpoint, có transaction) | 8/10 | 40% khớp |

Sai lệch tăng theo độ phức tạp — khớp với "sai lệch hiệu chuẩn độ tự tin" nêu trong bài: agent không giảm độ tự tin tương ứng khi tác vụ khó hơn.

## Luyện prompt (mẫu)

**Định nghĩa hoàn thành 3 cấp (`CLAUDE.md`):**
```markdown
Một tác vụ chỉ được coi là hoàn thành khi qua đủ 3 cấp, THEO ĐÚNG THỨ TỰ
(KHÔNG ĐƯỢC bỏ qua cấp trước dù cấp sau "có vẻ" sẽ pass):
1. Unit: `npm test -- <file liên quan>` pass.
2. Integration: `npm test -- tests/integration/` pass (test chạy với DB thật trong container).
3. End-to-end: kịch bản curl thật mô phỏng luồng người dùng đầy đủ, kiểm
   tra giá trị trả về chính xác — không chỉ status code 200.
Nếu cấp N fail, KHÔNG ĐƯỢC chạy hoặc báo cáo cấp N+1.
```

**Prompt evaluator độc lập ("kén chọn"):**
```
Bạn là evaluator độc lập, KHÔNG PHẢI người viết code này. Nhiệm vụ:
đánh giá xem tính năng "phạt trễ hạn" có thực sự hoàn thành không.

Bạn PHẢI kén chọn: chỉ duyệt (PASS) khi có bằng chứng runtime cụ thể —
output thật của lệnh xác minh cấp 3 (end-to-end), không chấp nhận báo
cáo dạng "đã test và hoạt động tốt" nếu không kèm log/output thật.
Nếu thiếu bằng chứng, trả về FAIL kèm chính xác bằng chứng nào còn thiếu.
Bạn không được tự chạy lại code để "giúp" generator sửa — chỉ đánh giá
dựa trên bằng chứng đã cung cấp.
```
