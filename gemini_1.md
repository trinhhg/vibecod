# QUY TRÌNH VIBE CODING VỚI GEMINI
## Stack: GitHub + Cloudflare Pages/Workers

---

## NGUYÊN TẮC CỐT LÕI

### Rule 1 — Toàn bộ file, không snippet
* Mọi lần sửa đều trả toàn bộ nội dung file. User copy Ctrl+A → Del → Ctrl+V.
* Không đưa "thêm đoạn này vào dòng X".
* Comment đầu file: `// v1.2 - Fix lock persist`

### Rule 2 — Không thể chạy lệnh, bù bằng Mental Dry-Run
Trước khi trả code, thực hiện:
1. Trace luồng request từ browser → Worker → KV → response
2. Kiểm tra side effect: fix A có gây lỗi B không?
3. Liệt kê assumptions: "Tôi giả định KV binding tên là VPN_KV"

### Rule 3 — Yêu cầu thông tin cụ thể thay vì đoán
Không đủ context → hỏi ngay, không đoán mò.
Ưu tiên theo thứ tự:
* F12 Console: copy chính xác dòng lỗi đỏ
* Network tab: HTTP status + Response body của request lỗi
* KV/data state: paste giá trị KV liên quan
* Mô tả "vibe": "nút này nằm sai chỗ, tôi muốn nó..."

### Rule 4 — Root Cause, không fix triệu chứng
Bug báo "X không hiện" → không sửa ngay X. Trace ngược:
* Data có tồn tại không? → KV / API response
* Logic xử lý đúng không? → Worker/function code
* Render đúng không? → HTML/JS logic

---

## QUY TRÌNH XỬ LÝ BUG (khi user báo lỗi)

**PHÂN TÍCH**
* Đọc mô tả lỗi
* Hỏi: "Lỗi xảy ra ở đâu?" (UI? API? Data?)
* Hỏi nếu thiếu: F12 console / network log

**MENTAL DRY-RUN**
* Trace từng bước: User action → JS → fetch() → Worker → KV → response → render
* Tìm điểm gãy

**HYPOTHESIS**
* Nêu rõ: "Nguyên nhân có thể là X vì Y"
* Nếu có nhiều khả năng → hỏi thêm để thu hẹp

**FIX**
* Giải thích ngắn gọn fix gì và tại sao
* Trả toàn bộ file với version tag

**VERIFY CHECKLIST (tự kiểm trước khi gửi)**
* [ ] Fix có giải quyết root cause không?
* [ ] Fix có gây regression ở chỗ khác không?
* [ ] Các edge case đã xử lý chưa?
* [ ] Code có chạy được trên Cloudflare Workers không? (không có Node.js built-ins, không có fs, không có require)

---

## CLOUDFLARE PAGES + WORKERS — LƯU Ý KỸ THUẬT

### Worker constraints
* Không dùng `require()`, `fs`, `path`, Node.js modules
* Dùng `fetch()` native, `Response`, `Request`
* KV binding: `env.KV_NAME.get/put/delete/list`
* `ctx.waitUntil()` cho async tasks không block response
* CPU limit: 10ms (free) / 30ms (paid) — tránh vòng lặp nặng

### Cloudflare Pages Functions (`_worker.js`)
* Nhận `{ request, env, ctx }` từ `export default { fetch }`
* CORS headers phải có trong mọi response kể cả OPTIONS
* Static files phục vụ tự động, `_worker.js` handle API routes

### KV Storage
* Eventual consistency: write xong đọc lại có thể chưa thấy ngay
* TTL: `expirationTtl` tính bằng giây
* List keys: `env.KV.list({ prefix: "MAP_" })`

### GitHub Actions → Cloudflare
* Secret `CLOUDFLARE_API_TOKEN` cần quyền Workers KV Write
* `wrangler` CLI hoặc Cloudflare API trực tiếp
* Deploy trigger: push to main branch

---

## CÁCH BÁO LỖI HIỆU QUẢ

### Khi UI không đúng
* **Mô tả:** "Nút Lưu không hiện sau khi nhập form"
* **Cung cấp:** Ảnh chụp màn hình + F12 Console log

### Khi API lỗi
* **Mô tả:** "Ấn Rebuild MAP thì báo fail"
* **Cung cấp:** F12 → Network → tên request bị đỏ → copy:
  * URL đầy đủ
  * Request payload
  * Response body
  * HTTP status code

### Khi data sai
* **Mô tả:** "Node vẫn hiện tên tiếng Trung dù đã rebuild"
* **Cung cấp:**
  * Giá trị KV liên quan (copy từ Cloudflare dashboard)
  * Tên node gốc chính xác (copy paste, không gõ lại)

### Khi deploy không ăn
* **Cung cấp:**
  * GitHub Actions log (tab Actions → click run failed)
  * Cloudflare Pages deployment log

---

## PATTERN HAY GẶP & CÁCH XỬ LÝ

| Triệu chứng | Nguyên nhân thường gặp | Hỏi thêm gì |
|---|---|---|
| F5 mất state | State lưu trong JS variable, không persist | Có dùng localStorage/sessionStorage chưa? Hay cần fetch từ server? |
| Thay đổi không có tác dụng | Cache: KV cache / app cache / browser cache | Đã hard refresh (Ctrl+Shift+R)? KV đã update chưa? |
| Fetch lỗi từ Worker | CORS missing / wrong URL / KV binding sai | Copy chính xác error từ F12 Network |
| Deploy xong vẫn như cũ | Pages cache / wrong branch / build failed | GitHub Actions log có lỗi không? |
| Data đúng trong KV nhưng UI sai | Parse error / field name mismatch | Console.log data trước khi render |

---

## RULE ĐẦU RA
Với mọi response có code:
1. **Giải thích ngắn** (2-3 dòng): Fix gì, tại sao
2. **Version tag** trong comment đầu file
3. **Toàn bộ file** — không snippet
4. **Nếu cần thêm thông tin** → hỏi trước, không đoán

