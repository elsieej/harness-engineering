# Bài mẫu tham khảo — KHÔNG PHẢI bài làm của bạn

Đây là các câu trả lời **mẫu** do Claude viết, để bạn thấy "một câu trả lời tốt trông như thế nào" theo đúng rubric ở `../thuc-hanh/DANH-GIA.md`. Đây **không phải** bài tập được làm hộ bạn.

## Dùng đúng cách (quan trọng)

1. Đọc bài giảng ở `../bai-giang/lecture-XX.md`.
2. **Tự viết câu trả lời của bạn trước**, vào `../thuc-hanh/bai-giang/lecture-XX/bai-tap.md` và `../bai-tap-prompt/answers/lecture-XX.md`.
3. **Sau đó** mới mở file tương ứng ở đây để đối chiếu — xem bạn thiếu gì, viết mơ hồ chỗ nào, hoặc làm tốt hơn cả mẫu ở đâu.
4. Nếu bạn đọc mẫu trước khi tự làm, bạn sẽ không luyện được kỹ năng thật — giống mở đáp án trước khi giải bài, chính là "tuyên bố hoàn thành sớm" mà bài 09 cảnh báo, chỉ là áp dụng cho việc học thay vì code.

## Về phần dự án (`du-an/`)

Các "nhật ký" trong `du-an/` **không phải số đo thật** — Claude không tự cài và chạy ứng dụng Electron của khoá học để đo thời gian thật. Chúng là **ví dụ minh hoạ** về hình dạng số liệu hợp lý (dùng tham chiếu gần với các con số Anthropic/OpenAI trích dẫn trong bài giảng gốc), để bạn biết nên ghi lại loại thông tin gì và kỳ vọng mức chênh lệch cỡ nào. Khi bạn tự chạy dự án thật, số của bạn chắc chắn sẽ khác — điều đó bình thường và đáng ghi lại.

## Dự án ví dụ dùng xuyên suốt

Mọi ví dụ ở bài giảng 01–12 dùng chung một dự án giả định: **LibraryAPI** — REST API quản lý thư viện sách nội bộ.
- Stack: Node.js 20 + Express + PostgreSQL 15 + Jest + ESLint
- Miền nghiệp vụ: sách (`/books`), độc giả (`/members`), phiếu mượn (`/loans`)
- Quy ước có sẵn: phân trang `?page=&limit=`, response `{ data, meta }` / lỗi `{ error: { code, message } }`

Dùng chung một dự án giúp bạn thấy harness "tích luỹ" qua từng bài — AGENTS.md bài 2 được tái cấu trúc ở bài 4, PROGRESS.md bài 5 được tham chiếu lại ở bài 6, feature list bài 7 được siết chặt hơn ở bài 8, v.v. — đúng tinh thần khoá học gốc (một ứng dụng Electron xuyên suốt 6 dự án).
