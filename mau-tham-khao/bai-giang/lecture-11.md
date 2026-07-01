# Bài mẫu tham khảo – Bài 11

> Ví dụ minh hoạ, không phải đáp án duy nhất. Đọc `../README.md` trước khi dùng file này.

## Bài tập 1 (mẫu)

Kiểm toán LibraryAPI theo 2 lớp quan sát:
- **Quan sát runtime**: có log request cơ bản (morgan), nhưng KHÔNG log giá trị tính toán trung gian (ví dụ số tiền phạt được tính ra sao) — khi có bug, không phân biệt được "tính sai công thức" với "dữ liệu đầu vào sai".
- **Quan sát quá trình**: hoàn toàn không có — không ai ghi lại "vì sao chấp nhận" một thay đổi, review dựa trên cảm tính người đọc code.

Trạng thái hệ thống không phân biệt được: khi `fine_amount` trả sai, không có cách nào (từ log hiện tại) biết lỗi nằm ở bước tính ngày trễ hay bước nhân đơn giá.

## Bài tập 2 (mẫu)

Sprint contract cho tính năng "xuất báo cáo mượn sách theo tháng (PDF)": chạy agent theo contract, so với không có contract — có contract: agent hỏi đúng 0 câu làm rõ phạm vi (đã có sẵn trong contract), review chỉ mất 1 vòng; không contract: agent tự quyết định thêm biểu đồ không được yêu cầu, review mất 2 vòng để cắt bớt phạm vi.

## Luyện prompt (mẫu)

**Sprint contract:**
```markdown
## Phạm vi
Thêm endpoint `GET /reports/loans/monthly?month=&year=` trả file PDF
liệt kê mọi phiếu mượn trong tháng, nhóm theo độc giả. KHÔNG bao gồm
biểu đồ, KHÔNG bao gồm gửi email tự động — đó là sprint sau.

## Tiêu chuẩn xác minh
- `npm test tests/routes/reports.test.ts` pass
- Gọi endpoint thật, mở file PDF trả về, xác nhận có đúng số dòng bằng
  số phiếu mượn thật trong tháng đó (đối chiếu query SQL độc lập)
- Thời gian phản hồi < 3s với 1000 phiếu mượn trong tháng

## Ngoại lệ
Nếu tháng được yêu cầu không có phiếu mượn nào: trả PDF 1 trang "Không
có dữ liệu", KHÔNG trả lỗi 404.
```

**Rubric chấm điểm (4 chiều, A-D):**
| Chiều | A | B | C | D |
|---|---|---|---|---|
| Đúng phạm vi | Đúng 100% phạm vi, không thêm/thiếu | Thiếu 1 chi tiết nhỏ đã nêu | Thiếu ngoại lệ | Làm sai/thêm ngoài phạm vi |
| Bằng chứng end-to-end | Có log gọi thật + PDF đính kèm đối chiếu số liệu khớp | Có log gọi thật, chưa đối chiếu số liệu | Chỉ có unit test | Không có bằng chứng runtime |
| Hiệu năng | Đo thật <3s, có số liệu | Ước lượng hợp lý, chưa đo | Không đề cập | Biết chậm nhưng không báo |
| Xử lý ngoại lệ | Test cả case rỗng, đúng như contract | Có xử lý nhưng chưa test | Không xử lý case rỗng | Trả lỗi sai (404) cho case rỗng |

Mỗi mức đều gắn bằng chứng cụ thể (log/số đo/test) — không chấp nhận "trông ổn".
