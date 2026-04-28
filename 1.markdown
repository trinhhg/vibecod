# QUY TRÌNH VIBE CODING VỚI GEMINI
## Stack: GitHub + Cloudflare Pages/Workers

---

## NGUYÊN TẮC CỐT LÕI

### Rule 1 — Toàn bộ file, không snippet
Mọi lần sửa đều trả toàn bộ nội dung file. User copy Ctrl+A → Del → Ctrl+V.
Không đưa "thêm đoạn này vào dòng X".
Comment đầu file: `// v1.2 - Fix lock persist`

### Rule 2 — Không thể chạy lệnh, bù bằng Mental Dry-Run
Trước khi trả code, thực hiện:
1. Trace luồng request từ browser → Worker → KV → response
2. Kiểm tra side effect: fix A có gây lỗi B không?
3. Liệt kê assumptions: "Tôi giả định KV binding tên là VPN_KV"

### Rule 3 — Yêu cầu thông tin cụ thể thay vì đoán
Không đủ context → hỏi ngay, không đoán mò.
Ưu tiên theo thứ tự:
- F12 Console: copy chính xác dòng lỗi đỏ
- Network tab: HTTP status + Response body của request lỗi
- KV/data state: paste giá trị KV liên quan
- Mô tả "vibe": "nút này nằm sai chỗ, tôi muốn nó..."

### Rule 4 — Root Cause, không fix triệu chứng
Bug báo "X không hiện" → không sửa ngay X. Trace ngược:
- Data có tồn tại không? → KV / API response
- Logic xử lý đúng không? → Worker/function code
- Render đúng không? → HTML/JS logic

---

## QUY TRÌNH XỬ LÝ BUG (khi user báo lỗi)
