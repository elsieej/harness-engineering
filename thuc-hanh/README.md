# Không gian thực hành (Homework & Project workspace)

Đây là nơi bạn **thực sự làm bài tập** — khác với `../bai-giang/` (chỉ đọc) và `../bai-tap-prompt/` (đề bài luyện prompt), thư mục này là chỗ bạn viết câu trả lời, chạy thí nghiệm, ghi log, và tự chấm trước khi nhờ Claude review.

## Cây thư mục

```
vn-hoc-harness/
├── bai-giang/lecture-01.md ... lecture-12.md   <- đề bài (đọc)
├── bai-tap-prompt/answers/lecture-01.md ...    <- bạn viết câu trả lời "Luyện prompt" ở đây
├── du-an/README.md                             <- tổng quan 6 dự án + cách so sánh baseline/harness
└── thuc-hanh/                                  <- BẠN ĐANG Ở ĐÂY
    ├── README.md                               <- file này: quy trình + cách dùng
    ├── DANH-GIA.md                              <- rubric tự chấm, theo từng bài giảng
    ├── bai-giang/
    │   ├── lecture-01/bai-tap.md               <- viết câu trả lời phần "Bài tập" của bài 01
    │   ├── lecture-02/bai-tap.md
    │   ... (đủ 12 bài)
    └── du-an/
        ├── project-01/nhat-ky.md               <- nhật ký chạy thí nghiệm dự án 01 (baseline vs harness)
        ├── project-02/nhat-ky.md
        ... (đủ 6 dự án)
```

## Quy trình từng bài giảng (lặp lại 12 lần, từ bài 01 → 12)

1. **Đọc** `../bai-giang/lecture-XX.md` (5-10 phút). Nếu muốn hiểu sâu, mở thêm bản đầy đủ chính thức (link GitHub/site ở đầu file).
2. **Làm "Bài tập"** — viết câu trả lời/số liệu/quan sát vào `thuc-hanh/bai-giang/lecture-XX/bai-tap.md`.
3. **Làm "Luyện prompt"** — viết artifact (AGENTS.md, feature list, PROGRESS.md...) vào `bai-tap-prompt/answers/lecture-XX.md`.
4. **Tự chấm nhanh** — đối chiếu cả hai với mục tương ứng trong `thuc-hanh/DANH-GIA.md`: đủ tiêu chí "Đạt" chưa? có mắc lỗi thường gặp không?
5. **Nhờ Claude review** — nhắn kiểu: *"review bài tập và luyện prompt lecture-03 của tôi"*. Claude sẽ áp dụng đúng rubric của bài đó (không phải nhận xét chung chung) và chỉ rõ: thiếu artifact nào, chỗ nào còn mơ hồ, lệnh xác minh có thực thi được không.
6. **Sửa lại** theo góp ý, rồi đánh dấu `[x]` vào bảng ở `../00-LO-TRINH.md`.

## Quy trình dự án thực hành (sau mỗi cặp 2 bài giảng)

Bảng ghép cặp bài giảng ↔ dự án nằm ở `../du-an/README.md`. Với mỗi dự án:

1. Lấy code khởi điểm: repo gốc đã clone ở `learn-harness-engineering/projects/project-0X/starter` (ở thư mục cha của `vn-hoc-harness/`), hoặc xem trên [GitHub](https://github.com/walkinglabs/learn-harness-engineering/tree/main/projects).
2. Tạo **2 bản làm việc** từ cùng một `starter/`: một bản **baseline** (chỉ prompt, không thêm gì), một bản **harness** (thêm artifact harness theo cột cuối bảng trong `du-an/README.md`).
3. Giao cùng một tác vụ, cùng một agent, cho cả hai bản. Đo: thời gian, số vòng lặp thử-sai, tỷ lệ tính năng pass xác minh end-to-end, chi phí nếu theo dõi được.
4. Ghi toàn bộ vào `thuc-hanh/du-an/project-0X/nhat-ky.md`.
5. Đối chiếu với `solution/` **sau khi** tự làm xong — để tham khảo, không sao chép trước.
6. Nhờ Claude review: *"review dự án 01 của tôi"* — Claude sẽ đối chiếu kết quả của bạn với dữ liệu Anthropic/OpenAI được trích trong bài giảng gốc (ví dụ: "tỷ lệ hoàn thành tăng 45%" ở bài 08) để đánh giá xem thí nghiệm của bạn có đo đúng thứ cần đo không.

## Đừng bỏ qua bước tự chấm

Rubric ở `DANH-GIA.md` không phải hình thức — mỗi bài giảng có 1-2 lỗi "bẫy" rất hay gặp (ví dụ bài 02: viết AGENTS.md thành cả một cuốn cẩm nang thay vì bản đồ ngắn). Tự chấm trước giúp bạn tiết kiệm một vòng review, và là chính bài tập "verification gap" mà bài 01 và bài 09 nói tới — đừng tự tuyên bố "xong" khi chưa đối chiếu tiêu chí khách quan.
