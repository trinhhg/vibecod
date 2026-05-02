# QUY TRÌNH LÀM VIỆC VỚI GEMINI — Vibe Coding

## Nguyên tắc cốt lõi

Gemini không có terminal. Không thể chạy lệnh, không thể đọc file thực tế.
Bù lại bằng **Mental Dry-Run** — chạy thử logic trong đầu trước khi viết code.

---

## Rule 1 — Luôn trả toàn bộ file (Total Overwrite)

- **KHÔNG snippet**, không "thêm đoạn này vào dòng X"
- Mọi sửa đổi → trả toàn bộ nội dung file
- Dòng comment đầu file: `// v1.2 - Fix tên bug - date`
- User thực hiện: Ctrl+A → Delete → Ctrl+V → Save → Deploy

## Rule 2 — Yêu cầu đúng thông tin, không đoán

Khi thiếu context → hỏi ngay, KHÔNG giả định:

| Loại lỗi | Cần user cung cấp |
|---|---|
| UI sai / trang trắng | F12 → Console → copy đỏ |
| API fail | F12 → Network → click request đỏ → copy Status + Response |
| KV data sai | Paste giá trị KV từ Cloudflare dashboard |
| Deploy không ăn | GitHub Actions log tab |

## Rule 3 — Mental Dry-Run trước khi viết code

Với mọi bug, thực hiện theo thứ tự:

```
1. ĐỌC: Đọc kỹ mô tả lỗi + code hiện tại
2. TRACE: Luồng browser → JS → fetch() → Worker → KV → response → render
3. TÌM: Điểm gãy chính xác ở đâu
4. HYPOTHESIS: "Lỗi ở X vì Y. Side effect Z có thể xảy ra."
5. FIX: Viết code fix, check không gây regression
6. VERIFY: Tự đặt câu hỏi — "Nếu user làm A, code có đúng không?"
```

## Rule 4 — Không rút gọn code không có lý do

Khi user paste code → KHÔNG tự ý xóa, rút gọn, hay refactor những phần không liên quan đến bug. Chỉ sửa đúng chỗ cần sửa.

## Rule 5 — Cấu trúc file dự án này

```
index.html      — Trang khách hàng (public)
admin.html      — Trang quản trị (password-protected JS)
_worker.js      — Cloudflare Pages Worker (API + KV logic)
update_sub.py   — GitHub Actions script (fetch + push KV)
```

Không tách nhỏ hơn. Không thêm file mới trừ khi user yêu cầu.

---

## Quy trình nhận bug report

```
User: "Tính năng X bị lỗi Y"
→ Gemini: đọc code liên quan → trace logic → nếu đủ info: fix
                                             → nếu thiếu: hỏi 1 câu duy nhất
```

Hỏi tối đa 1 câu/lần. Không hỏi nhiều câu cùng lúc.
