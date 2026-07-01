# Tổng quan 6 dự án thực hành

Cả 6 dự án đều tiến triển trên **cùng một ứng dụng**: một Electron knowledge-base app (React + TypeScript + Vite), nơi bạn import tài liệu, xem chi tiết, đánh index, và hỏi-đáp có trích dẫn (grounded Q&A). Mỗi dự án thêm một lớp harness mới lên trên sản phẩm của dự án trước. Bản dịch đầy đủ từng dự án: [GitHub](https://github.com/walkinglabs/learn-harness-engineering/tree/main/docs/vi/projects) · [Trang web chính thức](https://walkinglabs.github.io/learn-harness-engineering/vi/projects/). Code thật (starter/solution từng dự án): [GitHub](https://github.com/walkinglabs/learn-harness-engineering/tree/main/projects).

Stack chung: Node.js + TypeScript + React + Vite + Electron. Lệnh phổ biến trong mỗi `starter/` hoặc `solution/` (xem `package.json` của từng project để chắc chắn):

```bash
npm install        # cài dependency
npm run dev         # chạy app ở chế độ dev
npm run build        # build production
npm run check        # type-check (tsc --noEmit)
npm test            # chạy test (vitest)
```

## Bảng tổng quan

| Dự án | Tên | Bài giảng liên quan | Bạn làm gì | Harness liên quan |
|---|---|---|---|---|
| 01 | Chỉ Prompt vs. Ưu tiên Quy tắc | Bài 1-2 | Dựng Electron shell tối giản (danh sách tài liệu, panel Q&A, thư mục dữ liệu). Chạy 2 lần: không hỗ trợ vs. có `AGENTS.md`+`init.sh`+`feature_list.json`. | `AGENTS.md`, `CLAUDE.md`, `init.sh`, `feature_list.json`, `claude-progress.md` |
| 02 | Không gian làm việc Agent đọc được | Bài 3-4 | Thêm import tài liệu, trang chi tiết, local persistence qua 2 phiên. So sánh có/không có `ARCHITECTURE.md`, `PRODUCT.md`, `session-handoff.md`. | Tài liệu kiến trúc đặt cạnh code, tệp bàn giao phiên |
| 03 | Tính liên tục đa phiên | Bài 5-6 | Document chunking, metadata extraction, tiến độ indexing, Q&A có trích dẫn. Dùng `feature_list.json` để không đánh dấu "passing" thiếu bằng chứng. | Feature list có trạng thái, restart/khởi tạo có kiểm soát |
| 04 | Phản hồi Runtime & Kiểm soát Phạm vi | Bài 7-8 | Thêm observability (log khởi động, import/indexing), ràng buộc kiến trúc chống vi phạm xuyên lớp. Có cài sẵn 1 lỗi runtime để agent tự sửa. | Log runtime, lint kiến trúc, feature list làm cổng phạm vi |
| 05 | Tự xác minh & Phân tách Vai trò | Bài 9-10 | Triển khai generator/evaluator (và tuỳ chọn planner). Chạy 3 lần để đo tác động của từng vai trò thêm vào, trên một tính năng nâng cấp tự chọn. | Kiến trúc generator-evaluator độc lập, xác nhận 3 lớp |
| 06 | Harness Đầy đủ (Capstone) | Bài 11-12 | Lắp ráp toàn bộ: chạy benchmark với harness yếu → harness mạnh nhất → dọn dẹp → chạy lại → thí nghiệm ablation (xoá từng thành phần, xem cái nào thật sự quan trọng). | Toàn bộ 5 hệ thống phụ + observability + clean-state checklist |

## Cách chạy so sánh "có harness / không harness"

Mẫu chung cho hầu hết các dự án:
1. Tạo 2 branch/thư mục làm việc từ cùng một `starter/`.
2. Nhánh A (baseline): chỉ đưa `task-prompt.md` cho agent, không thêm gì khác.
3. Nhánh B (harness): thêm các artifact harness được liệt kê ở cột cuối bảng trên trước khi giao việc.
4. Cùng một agent (Claude Code hoặc Codex), cùng prompt gốc, đo: thời gian, số vòng lặp thử-sai, tỷ lệ tính năng pass xác minh end-to-end, chi phí (nếu theo dõi được).
5. Đối chiếu với `solution/` để biết kết quả tham chiếu — nhưng mục tiêu chính là quan sát *sự khác biệt về hành vi và kết quả* giữa hai lần chạy, không phải sao chép solution.

## Gợi ý tiến độ

Chạy song song với `../bai-giang/`: hoàn thành 2 bài giảng liên quan trước, rồi làm dự án tương ứng — lý thuyết cho bạn biết "vì sao", dự án cho bạn cảm nhận "khác biệt lớn đến đâu" bằng chính tay mình.
