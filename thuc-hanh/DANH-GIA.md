# Rubric tự chấm (và Claude sẽ chấm theo đúng tiêu chí này)

Đây không phải rubric chung chung ("viết rõ ràng", "đầy đủ") — mỗi bài giảng có tiêu chí riêng, rút thẳng từ khái niệm cốt lõi của bài đó. Khi bạn nhờ Claude review, Claude áp dụng đúng mục tương ứng dưới đây, không phải nhận xét cảm tính.

## Thang điểm chung

- **0 – Chưa đạt**: thiếu thành phần bắt buộc, hoặc chỉ là mô tả chung chung không thực thi được (đúng lỗi mà bài 01/09 cảnh báo: "trông ổn" không phải bằng chứng).
- **1 – Đạt cơ bản**: đủ thành phần yêu cầu, nhưng còn mơ hồ hoặc thiếu tính thực thi ở ít nhất một chỗ.
- **2 – Tốt**: đủ thành phần, cụ thể, thực thi được (agent mới đọc vào là làm được ngay, không cần hỏi lại).
- **3 – Xuất sắc**: như mức 2, cộng thêm xử lý được một tình huống biên mà bài giảng gốc có nhắc tới (ví dụ: ablation, race condition khi nhiều agent chạy song song, khác biệt theo model...).

Mục tiêu không phải đạt điểm 3 ở mọi bài — mà là **biết chính xác vì sao** một câu trả lời rơi vào mức nào, vì đó chính là kỹ năng harness engineering thật.

---

## Bài 01 — Mô hình mạnh không có nghĩa là thực thi đáng tin cậy

**Tiêu chí đạt (Luyện prompt — Definition of Done):**
- Nêu cụ thể endpoint/giao diện cần có (không phải "thêm tìm kiếm" chung chung)
- Hành vi mong đợi được viết thành câu có thể đúng/sai rõ ràng
- Có ≥1 lệnh xác minh thực thi được (test, type-check, lint) — không phải "kiểm tra thủ công"

**Lỗi thường gặp:** viết DoD vẫn mơ hồ như prompt gốc; không có lệnh xác minh nào chạy được; nhầm "mô tả tính năng" với "tiêu chí hoàn thành".

**Tiêu chí đạt (Bài tập):** phân loại lỗi đúng vào 1 trong 5 lớp (đặc tả/ngữ cảnh/môi trường/xác minh/trạng thái) chứ không gộp chung "agent bị lỗi"; có tính ra được tỷ lệ % agent tự báo "xong" sai (khoảng cách xác minh).

## Bài 02 — Harness thực sự là gì

**Tiêu chí đạt (Luyện prompt — AGENTS.md/CLAUDE.md):**
- Có đủ dấu vết của **cả 5 hệ thống phụ**: hướng dẫn, công cụ, môi trường, trạng thái, phản hồi — thiếu 1 là chưa đạt
- Độ dài ≤ 100 dòng (đây là bài kiểm tra chính — "bản đồ, không phải cẩm nang")
- Ràng buộc cứng ≤ 10-15 điều, viết dạng thực thi được, không liệt kê chi tiết từng bước

**Lỗi thường gặp:** viết thành tài liệu >150 dòng bao trùm mọi convention (chính là lỗi bài 04 cảnh báo trước); thiếu hẳn phần "trạng thái" hoặc "phản hồi" vì tưởng chỉ cần hướng dẫn + công cụ.

## Bài 03 — Biến kho lưu trữ thành nguồn sự thật duy nhất

**Tiêu chí đạt (Luyện prompt — ARCHITECTURE.md/CONSTRAINTS.md):**
- Dùng ngôn ngữ PHẢI/KHÔNG ĐƯỢC rõ ràng, không phải gợi ý mềm ("nên dùng...")
- Đặt ở vị trí một agent mới sẽ thực sự đọc được (cạnh code liên quan), không chôn trong tài liệu ít ai mở
- Tưởng tượng "bài kiểm tra phiên mới": nếu chỉ đưa repo cho agent lạ, nó có suy ra được ràng buộc này không?

**Lỗi thường gặp:** viết ràng buộc dạng đề xuất thay vì luật cứng; đặt đúng nội dung nhưng sai vị trí (agent sẽ không tìm thấy khi cần).

## Bài 04 — Chia hướng dẫn ra thành nhiều tệp

**Tiêu chí đạt (Luyện prompt — kế hoạch tái cấu trúc):**
- Tệp đầu vào rút gọn còn ≤ 15 ràng buộc cứng, có tên cụ thể (không phải "giữ lại phần quan trọng")
- Liệt kê tên + độ dài ước lượng cho từng tài liệu chủ đề sẽ tách ra
- Có quyết định rõ: nội dung nào **xoá hẳn** hoặc chuyển thành test case — không phải "giữ hết, chỉ sắp xếp lại"

**Lỗi thường gặp:** tái cấu trúc nhưng tổng nội dung không giảm (chỉ đổi chỗ, không cắt); không áp dụng "reveal on demand" — vẫn nhét chi tiết vào tệp đầu vào.

## Bài 05 — Duy trì ngữ cảnh xuyên suốt các phiên

**Tiêu chí đạt (Luyện prompt — PROGRESS.md + DECISIONS.md):**
- PROGRESS.md có đủ: trạng thái hiện tại (commit/test pass-fail cụ thể), đã hoàn thành, đang thực hiện, vấn đề đã biết, bước tiếp theo
- DECISIONS.md ghi rõ **lý do** và **phương án bị loại** — không chỉ ghi "đã chọn X"
- Một phiên mới hoàn toàn đọc xong có thể tiếp tục việc trong <3 phút (đây là chỉ số "chi phí tái thiết lập" của bài)

**Lỗi thường gặp:** PROGRESS.md chỉ có "đang làm tính năng X" (quá mơ hồ, không có trạng thái commit/test cụ thể); DECISIONS.md không ghi phương án bị loại nên phiên sau không biết vì sao không chọn cách khác.

## Bài 06 — Khởi tạo trước mỗi phiên agent

**Tiêu chí đạt (Luyện prompt — prompt "chỉ khởi tạo"):**
- **Cấm rõ ràng** việc viết code tính năng nghiệp vụ (không phải ngụ ý, phải viết thẳng)
- Yêu cầu đủ 4 sản phẩm: môi trường chạy được, ≥1 test mẫu pass, tài liệu danh sách sẵn sàng, bản phân rã ≥3 mục có tiêu chí chấp nhận
- Có kèm checklist chấp nhận cụ thể để xác nhận phiên khởi tạo đã đạt

**Lỗi thường gặp:** để hé "có thể viết thêm vài dòng code nếu cần" (làm mất mục đích tách giai đoạn); thiếu tiêu chí chấp nhận cho từng mục phân rã.

## Bài 07 — Vạch ranh giới tác vụ rõ ràng cho agent

**Tiêu chí đạt (Luyện prompt — quy tắc WIP=1 + feature-list):**
- Quy tắc WIP=1 được viết dưới dạng **thực thi được** (agent tự kiểm tra được, không chỉ là lời nhắc)
- 3 mục feature-list mẫu đều có đủ 3 trường: id, lệnh xác minh thực thi được (ví dụ `curl ... | jq ...`), trạng thái
- Ngăn được agent tự nhận "xong" khi chưa qua lệnh xác minh

**Lỗi thường gặp:** lệnh xác minh viết mơ hồ ("kiểm tra bằng tay", "test thử") thay vì lệnh chạy được thật; quy tắc WIP=1 chỉ là câu nhắc nhở, không có cơ chế chặn.

## Bài 08 — Sử dụng feature list để ràng buộc agent

**Tiêu chí đạt (Luyện prompt — hiệu chỉnh độ hạt + quy tắc gating):**
- Cả tính năng "quá rộng" và "quá hẹp" đều được viết lại ở độ hạt "hoàn thành trong một phiên" — không còn quá rộng, không vụn vặt vô nghĩa
- Mỗi mục có lệnh xác minh cụ thể, thực thi được
- Có quy tắc rõ: chỉ script xác minh được phép chuyển trạng thái sang `passing`, agent không được tự sửa

**Lỗi thường gặp:** "quá rộng" chỉ được rút gọn câu chữ chứ chưa thực sự chia nhỏ (vẫn không xong trong 1 phiên); thiếu quy tắc cấm agent tự sửa trạng thái (đây là điểm quan trọng nhất của bài).

## Bài 09 — Ngăn chặn agent tuyên bố thành công quá sớm

**Tiêu chí đạt (Luyện prompt — DoD 3 cấp + evaluator "kén chọn"):**
- Có đúng **3 cấp** xác minh (unit → integration → end-to-end), với quy tắc cấm chuyển cấp khi cấp trước fail
- Prompt evaluator độc lập với generator — không tự chấm bài của chính nó
- Evaluator chỉ duyệt khi có **bằng chứng runtime cụ thể**, cấm chấp nhận "trông có vẻ ổn"

**Lỗi thường gặp:** chỉ có 2 cấp (unit + "test kỹ hơn") thay vì đủ 3 cấp phân biệt rõ; evaluator prompt không đủ "kén chọn" — vẫn chấp nhận khẳng định của generator mà không đòi bằng chứng.

## Bài 10 — Chỉ kiểm thử toàn bộ pipeline mới là xác minh thật

**Tiêu chí đạt (Luyện prompt — quy tắc kiến trúc + lint check):**
- Quy tắc viết dạng PHẢI/KHÔNG ĐƯỢC, nhắm vào một ranh giới kiến trúc cụ thể (không phải nguyên tắc chung "code sạch")
- Có script/lint pseudocode khả thi để tự động phát hiện vi phạm
- Thông báo lỗi đủ **3 phần**: sai ở đâu / vì sao / sửa thế nào — thiếu phần nào cũng chưa đạt

**Lỗi thường gặp:** quy tắc kiến trúc không thể tự động hoá được (chỉ kiểm tra được bằng mắt); thông báo lỗi chỉ nói "sai rồi", thiếu hướng dẫn sửa cụ thể.

## Bài 11 — Làm cho runtime của agent có thể quan sát được

**Tiêu chí đạt (Luyện prompt — sprint contract + rubric):**
- Sprint contract có đủ 3 phần: Phạm vi / Tiêu chuẩn xác minh / Ngoại lệ, viết cụ thể cho tính năng đã chọn
- Rubric có 3-4 chiều, mỗi chiều có đủ 4 mức A-D
- Mỗi mức trong rubric gắn với **bằng chứng cụ thể** (số đo, log, ảnh chụp) — không chấp nhận tính từ mơ hồ như "tốt"/"ổn"

**Lỗi thường gặp:** rubric chỉ là thang tính từ (A=xuất sắc, B=tốt...) không gắn bằng chứng cụ thể nào; sprint contract thiếu phần "Ngoại lệ" nên không biết khi nào được phép lệch phạm vi.

## Bài 12 — Bàn giao sạch ở cuối mỗi phiên

**Tiêu chí đạt (Luyện prompt — checklist thoát phiên + script dọn dẹp):**
- Checklist phủ đủ **5 điều kiện**: build pass, test pass, tiến độ đã ghi vào artifact máy đọc được, không còn artifact tạm, đường khởi động chuẩn vẫn chạy được
- Script dọn dẹp thực sự **idempotent** — chạy lại nhiều lần không sinh side effect mới, không lỗi nếu đã dọn rồi

**Lỗi thường gặp:** thiếu điều kiện "đường khởi động chuẩn vẫn chạy được" (hay bị quên nhất); script dọn dẹp không idempotent — ví dụ append log thay vì kiểm tra tồn tại trước, hoặc lỗi nếu file đã bị xoá.

---

## Rubric cho 6 dự án thực hành

Áp dụng chung cho mọi dự án (điều chỉnh theo cặp bài giảng liên quan ở `../du-an/README.md`):

- **Có đo được, không chỉ cảm nhận**: nhật ký phải có số liệu (thời gian, số vòng lặp, tỷ lệ pass) cho cả 2 nhánh, không chỉ nhận xét "nhánh harness tốt hơn".
- **So sánh đúng biến**: chỉ thay đổi harness giữa 2 nhánh, giữ nguyên model/prompt gốc — nếu không, không kết luận được gì (đây chính là điểm mà thí nghiệm Anthropic trong các bài giảng luôn kiểm soát).
- **Đối chiếu với dữ liệu gốc**: nhận xét có khớp (hoặc khác) với con số Anthropic/OpenAI nêu trong bài giảng liên quan không — nếu khác nhiều, đó là phát hiện đáng ghi lại, không phải lỗi cần giấu.
- **Dự án 06 (capstone)**: thêm yêu cầu chạy ablation — gỡ từng thành phần harness, đo mức giảm, để xác nhận (hoặc bác bỏ) trực giác đã xây dựng từ bài 01-11.
