# Lộ trình học Harness Engineering (bản tóm tắt tiếng Việt)

> Bản dịch chính thức đầy đủ (12 bài giảng + 6 dự án, do nhóm Walking Labs dịch) nằm ở repo gốc: [github.com/walkinglabs/learn-harness-engineering](https://github.com/walkinglabs/learn-harness-engineering/tree/main/docs/vi), và cũng xem online tại https://walkinglabs.github.io/learn-harness-engineering/vi/. Tài liệu bạn đang đọc **không thay thế** bản gốc, nó chỉ là bản tóm tắt cá nhân kèm bài tập và bài luyện prompt, để bạn học nhanh và thực hành có phản hồi.

## Ý tưởng cốt lõi của cả khoá học

Một mô hình AI giỏi không tự động cho ra kết quả đáng tin cậy. Cùng một model (ví dụ Opus 4.5), cùng một prompt, nhưng chạy trần trụi không có gì hỗ trợ thì thất bại, còn chạy trong một "harness" (bộ khung: hướng dẫn + công cụ + môi trường + trạng thái + phản hồi xác minh) được thiết kế tốt thì ra sản phẩm dùng được thật sự. Nói ngắn gọn: **khi có sự cố, đừng vội đổi model — hãy kiểm tra harness trước.** Cả khoá học là một chuỗi 12 bài giảng đi từ "vì sao" đến "làm thế nào" để xây dựng harness đó, và 6 dự án thực hành để bạn tự tay trải nghiệm sự khác biệt.

## Bảng lộ trình

| # | Tên bài học | Câu hỏi cốt lõi | Dự án thực hành | Trạng thái |
|---|---|---|---|---|
| 1 | Mô hình mạnh không có nghĩa là thực thi đáng tin cậy | Vì sao agent mạnh vẫn thất bại trên tác vụ thực tế? | Dự án 01 | [ ] |
| 2 | Harness thực sự có nghĩa là gì | Harness gồm những hệ thống phụ nào? | Dự án 01 | [ ] |
| 3 | Biến kho lưu trữ thành nguồn sự thật duy nhất | Vì sao "repo là spec", agent không thấy thì coi như không tồn tại? | Dự án 02 | [ ] |
| 4 | Chia hướng dẫn ra thành nhiều tệp | Vì sao một AGENTS.md khổng lồ lại phản tác dụng? | Dự án 02 | [ ] |
| 5 | Duy trì ngữ cảnh xuyên suốt các phiên | Vì sao tác vụ dài mất tính liên tục giữa các phiên? | Dự án 03 | [ ] |
| 6 | Khởi tạo trước mỗi phiên agent | Vì sao khởi tạo môi trường cần một giai đoạn riêng, tách khỏi code tính năng? | Dự án 03 | [ ] |
| 7 | Vạch ranh giới tác vụ rõ ràng cho agent | Vì sao agent hay "ôm đồm" nhiều việc cùng lúc mà chẳng xong việc nào? | Dự án 04 | [ ] |
| 8 | Sử dụng feature list để ràng buộc agent | Vì sao feature list là "nguyên thuỷ" chứ không phải ghi chú? | Dự án 04 | [ ] |
| 9 | Ngăn chặn agent tuyên bố thành công quá sớm | Vì sao agent luôn tự tin thái quá về việc mình đã "xong"? | Dự án 05 | [ ] |
| 10 | Chỉ kiểm thử toàn bộ pipeline mới là xác minh thật | Vì sao unit test xanh vẫn không chứng minh được điều gì? | Dự án 05 | [ ] |
| 11 | Làm cho runtime của agent có thể quan sát được | Vì sao thiếu observability khiến việc thử lại thành đoán mù? | Dự án 06 | [ ] |
| 12 | Bàn giao sạch ở cuối mỗi phiên | Vì sao mỗi phiên phải kết thúc ở trạng thái sạch? | Dự án 06 | [ ] |

Ghi chú vào cột "Trạng thái" khi bạn xong: `[x]` cho bài đã đọc + làm bài tập, thêm `(prompt: xong)` nếu đã hoàn thành cả phần luyện prompt.

## Nhịp độ gợi ý (4–6 tuần)

- **Tuần 1**: Bài 1–2 + Dự án 01 (Chỉ prompt vs. có harness cơ bản). Đây là bài "khai tâm" — làm chậm, làm kỹ, vì mọi bài sau đều dựa trên trực giác bạn xây ở đây.
- **Tuần 2**: Bài 3–4 + Dự án 02 (không gian làm việc agent đọc được).
- **Tuần 3**: Bài 5–6 + Dự án 03 (tính liên tục đa phiên).
- **Tuần 4**: Bài 7–8 + Dự án 04 (phản hồi runtime, kiểm soát phạm vi).
- **Tuần 5**: Bài 9–10 + Dự án 05 (tách vai trò generator/evaluator).
- **Tuần 6**: Bài 11–12 + Dự án 06 (capstone — lắp ráp toàn bộ, chạy thí nghiệm ablation).

Nếu bạn học nhanh hơn (2–3 buổi/tuần, mỗi buổi 1–2 tiếng), có thể nén còn 3–4 tuần. Đừng bỏ qua phần "Luyện prompt" — đó là phần biến kiến thức thụ động thành kỹ năng thật.

## Cách dùng thư mục này

```
vn-hoc-harness/
├── 00-LO-TRINH.md          <- bạn đang ở đây
├── bai-giang/              <- 12 file tóm tắt + đề bài tập + luyện prompt
├── du-an/README.md         <- tổng quan 6 dự án thực hành, cách chạy
├── bai-tap-prompt/
│   ├── README.md           <- danh sách toàn bộ 12 bài luyện prompt
│   └── answers/            <- nơi bạn viết câu trả lời để nhờ review
├── thuc-hanh/              <- KHÔNG GIAN LÀM BÀI: bài tập + nhật ký dự án + rubric tự chấm
│   ├── README.md           <- quy trình từng bước, chi tiết hơn phần dưới đây
│   ├── DANH-GIA.md          <- rubric tự chấm theo từng bài giảng/dự án
│   ├── bai-giang/lecture-01/bai-tap.md ...  <- viết câu trả lời phần "Bài tập"
│   └── du-an/project-01/nhat-ky.md ...      <- nhật ký so sánh baseline vs harness
└── mau-tham-khao/          <- BÀI MẪU (chỉ xem SAU KHI đã tự làm ở thuc-hanh/) — xem README ở đó
```

**Bắt đầu ngay**: mở `bai-giang/lecture-01.md`, đọc phần tóm tắt (5-10 phút). Rồi mở `thuc-hanh/README.md` để theo đúng quy trình 6 bước: làm bài tập → làm luyện prompt → tự chấm theo `thuc-hanh/DANH-GIA.md` → nhờ Claude review → sửa → đánh dấu hoàn thành ở bảng trên.
