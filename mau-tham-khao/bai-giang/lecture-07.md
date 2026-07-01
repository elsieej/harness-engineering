# Bài mẫu tham khảo – Bài 07

> Ví dụ minh hoạ, không phải đáp án duy nhất. Đọc `../README.md` trước khi dùng file này.

## Bài tập 1 (mẫu)

Yêu cầu rộng: "triển khai hệ thống quản lý phạt trễ hạn cho LibraryAPI". Phân rã ≥5 đơn vị nguyên tử:

| id | Hành vi | Lệnh xác minh | Phụ thuộc |
|---|---|---|---|
| fine-01 | Tính số ngày trễ hạn khi trả sách | `npm test tests/services/fine.calc.test.ts` | không |
| fine-02 | Endpoint `GET /loans/:id/fine` trả số tiền phạt | `curl localhost:3000/loans/1/fine \| jq .data.amount` | fine-01 |
| fine-03 | Migration thêm cột `fine_amount` vào `loans` | `npm run db:migrate` chạy không lỗi + kiểm tra schema | không |
| fine-04 | Job tự động đánh dấu quá hạn hàng ngày | `npm test tests/jobs/overdue.test.ts` | fine-03 |
| fine-05 | Endpoint thanh toán phạt `POST /loans/:id/pay-fine` | `npm test tests/routes/payFine.test.ts` | fine-02 |

Mỗi đơn vị hoàn thành được trong một phiên — thoả WIP=1.

## Bài tập 2 (mẫu)

Chạy 2 lần trên cùng 5 tác vụ trên: (A) không ràng buộc — agent mở cả 5 file cùng lúc, cuối phiên 3/5 chạy nhưng 0/5 pass test end-to-end vì đụng độ giữa fine-01 và fine-04 (đổi cùng hàm tính ngày). (B) WIP=1 thực thi — agent làm lần lượt, mỗi cái commit + test pass trước khi mở cái tiếp theo: 5/5 pass, tổng dòng code ít hơn ~15% (không có code viết rồi bỏ dở).

## Luyện prompt (mẫu) — Quy tắc WIP=1 + feature-list

```markdown
## Quy tắc làm việc (CLAUDE.md)

PHẢI: chỉ có đúng 1 mục feature-list ở trạng thái `active` tại một thời
điểm. KHÔNG ĐƯỢC bắt đầu mục tiếp theo khi mục hiện tại chưa `passing`.
KHÔNG ĐƯỢC tự đánh dấu một mục là `passing` — chỉ được chuyển sau khi
lệnh xác minh của chính mục đó chạy và trả về thành công.
```

```json
[
  {
    "id": "fine-01",
    "hanh_vi": "Tính đúng số ngày trễ hạn dựa trên due_date và ngày trả",
    "lenh_xac_minh": "npm test tests/services/fine.calc.test.ts",
    "trang_thai": "not_started"
  },
  {
    "id": "fine-02",
    "hanh_vi": "GET /loans/:id/fine trả amount = số_ngày_trễ * 5000",
    "lenh_xac_minh": "curl -s localhost:3000/loans/1/fine | jq -e '.data.amount == 15000'",
    "trang_thai": "not_started"
  },
  {
    "id": "fine-03",
    "hanh_vi": "Migration thêm cột fine_amount (integer, default 0) vào loans",
    "lenh_xac_minh": "npm run db:migrate && psql -c \"\\d loans\" | grep fine_amount",
    "trang_thai": "not_started"
  }
]
```

Với cấu trúc này, agent không thể "làm cho có" — mỗi mục chỉ được coi là xong khi đúng một lệnh cụ thể chạy pass, và không thể mở việc tiếp theo trong lúc việc hiện tại chưa qua cổng đó.
