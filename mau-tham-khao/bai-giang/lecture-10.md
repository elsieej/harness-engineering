# Bài mẫu tham khảo – Bài 10

> Ví dụ minh hoạ, không phải đáp án duy nhất. Đọc `../README.md` trước khi dùng file này.

## Bài tập 1 (mẫu)

Tác vụ chạm 3 component: route handler → service → repository, cho tính năng "trả sách trễ hạn tự động tính phạt". Unit test (mock từng layer): tất cả pass. Chạy end-to-end (DB thật + HTTP thật): phát hiện thêm 2 lỗi — (1) route trả `fine_amount` dạng string trong khi frontend giả định number (khiếm khuyết ranh giới component: cả route test và service test đều mock nên không ai bắt được); (2) transaction service mở nhưng repository dùng connection khác pool nên không rollback đúng khi lỗi giữa chừng.

Phân loại: lỗi (1) = không khớp giao diện; lỗi (2) = vấn đề vòng đời tài nguyên/phụ thuộc môi trường (connection pool).

## Bài tập 2 (mẫu)

Ràng buộc kiến trúc chọn: "route handler KHÔNG ĐƯỢC gọi trực tiếp `pg` — phải qua repository layer". Biến thành kiểm tra tự động:
```bash
grep -rn "require('pg')\|from 'pg'" src/routes/ && exit 1 || exit 0
```
Tích hợp vào `npm run lint:architecture`, chạy trong CI trước merge.

## Luyện prompt (mẫu)

**Quy tắc PHẢI/KHÔNG ĐƯỢC:**
```markdown
KHÔNG ĐƯỢC: route handler (src/routes/*) truy cập trực tiếp module `pg`
hoặc chạy raw SQL. Mọi truy vấn PHẢI đi qua src/repositories/*.
```

**Script kiểm tra (pseudocode grep/lint):**
```bash
#!/bin/bash
# scripts/lint-architecture.sh
violations=$(grep -rln "require('pg')\|from 'pg'\|import.*pg" src/routes/)
if [ -n "$violations" ]; then
  echo "VI PHẠM RÀNG BUỘC KIẾN TRÚC"
  echo "Sai ở đâu: $violations"
  echo "Vì sao: route handler không được truy cập DB trực tiếp — phá vỡ"
  echo "  ranh giới layer, khiến việc mock/test và đổi DB sau này khó khăn."
  echo "Sửa thế nào: chuyển logic truy vấn vào src/repositories/<domain>.ts,"
  echo "  route handler chỉ gọi hàm repository đã export."
  exit 1
fi
exit 0
```
Thông báo lỗi có đủ 3 phần (sai ở đâu / vì sao / sửa thế nào) — đúng mẫu bài giảng, giúp agent tự sửa mà không cần hỏi lại.
