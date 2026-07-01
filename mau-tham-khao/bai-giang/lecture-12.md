# Bài mẫu tham khảo – Bài 12

> Ví dụ minh hoạ, không phải đáp án duy nhất. Đọc `../README.md` trước khi dùng file này.

## Bài tập 1 (mẫu)

Checklist thoát phiên áp dụng qua 5 phiên liên tiếp trên LibraryAPI, ghi vi phạm theo chiều:

| Phiên | Build pass | Test pass | Tiến độ ghi lại | Không artifact tạm | Đường khởi động OK |
|---|---|---|---|---|---|
| 1 | ✅ | ✅ | ✅ | ✅ | ✅ |
| 2 | ✅ | ✅ | ❌ (quên cập nhật PROGRESS.md) | ✅ | ✅ |
| 3 | ✅ | ✅ | ✅ | ❌ (để lại `console.log` debug) | ✅ |
| 4 | ✅ | ✅ | ✅ | ✅ | ✅ |
| 5 | ✅ | ⚠️ (1 test skip tạm) | ✅ | ✅ | ✅ |

2/5 phiên vi phạm ít nhất 1 điều kiện — đúng như bài giảng cảnh báo: mà không chủ động kiểm, rác tích luỹ dần.

## Bài tập 2 (mẫu)

Thành phần chọn: bước "phải viết migration riêng cho mọi thay đổi schema" (từ AGENTS.md bài 02). Tạm vô hiệu hoá (cho phép sửa tay DB), chạy benchmark 5 tác vụ có đổi schema: nhanh hơn ngắn hạn (tiết kiệm ~5 phút/tác vụ) nhưng phiên sau bị lệch schema giữa máy local và staging 2/5 lần. **Quyết định: giữ**, vì chi phí sự cố lệch schema lớn hơn nhiều thời gian tiết kiệm được.

## Luyện prompt (mẫu)

**Checklist thoát phiên (`CLAUDE.md`):**
```markdown
## Danh sách kiểm tra thoát phiên (bắt buộc trước khi kết thúc)
1. `npm run build` pass
2. `npm test` pass (không có test bị skip tạm thời)
3. PROGRESS.md đã cập nhật: trạng thái/đã xong/đang làm/bước tiếp theo
4. Không còn artifact tạm: không `console.log` debug, không code
   comment-out, không TODO chưa được ghi vào feature list
5. `npm run dev` (đường khởi động chuẩn) vẫn chạy được từ đầu
```

**Script dọn dẹp idempotent (pseudocode bash):**
```bash
#!/bin/bash
# scripts/cleanup.sh — an toàn chạy lại nhiều lần
set -e

# Xoá console.log debug (chỉ những dòng có marker rõ ràng, tránh xoá nhầm log hợp lệ)
grep -rl "// DEBUG-TEMP" src/ | xargs -r sed -i '' '/\/\/ DEBUG-TEMP/d' || true

# Xoá code comment-out có marker, không lỗi nếu không tìm thấy file nào
find src/ -name "*.bak" -delete 2>/dev/null || true

# Kiểm tra lại, không sinh side effect nếu đã sạch từ trước
if grep -rq "// DEBUG-TEMP" src/; then
  echo "Vẫn còn debug marker, kiểm tra lại thủ công"; exit 1
fi
echo "Đã sạch."
```
Idempotent vì: dùng `|| true`/`2>/dev/null || true` để không lỗi khi không còn gì để xoá, và bước kiểm tra cuối xác nhận trạng thái thay vì giả định đã xoá thành công.
