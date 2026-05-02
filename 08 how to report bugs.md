# CÁCH BÁO LỖI HIỆU QUẢ — Cho Gemini chuẩn đoán đúng

## Nguyên tắc: Gemini không thể chạy code → cần data thực tế từ bạn

---

## Khi UI sai / trang trắng

**Cần:** Screenshot F12 Console (tab Console, lọc Errors)
```
Copy chính xác dòng đỏ:
TypeError: Cannot read properties of undefined (reading 'map')
  at renderLinks (admin.html:595)
```

## Khi API call thất bại

**Cần:** F12 → Network → click request bị đỏ → copy:
```
URL:      https://vpntest-ad4.pages.dev/api/links
Method:   POST
Status:   500
Response: {"error": "KV binding not found"}
```

## Khi node rename không đúng

**Cần:** Copy paste chính xác tên node trong app (KHÔNG gõ lại):
```
Tên trong app:  • 日本东京1号 - VPN Trinh Hg     ← copy từ ClashVerge
Tên muốn đổi:  🇯🇵 JP Tokyo 01 - VPN Trinh Hg
```
> Lý do: Emoji và ký tự Unicode trông giống nhau nhưng khác code point → rename dict sai không match

## Khi KV data bất thường

**Cần:** Paste giá trị KV từ Cloudflare Dashboard → Workers & Pages → KV:
```
MAP_9b3935d6:      9b3935d6fc66323c9c43a67fcc1dfcd2
ORIG_URL_9b3935d6: https://www.djjc.cfd/api/v1/...
SUB_DATA_9b3935d6: {"body_b64":"dmxlc3M6Ly8w..."} ← độ dài?
```

## Khi GitHub Actions bị lỗi

**Cần:** Tab Actions → click run đỏ → copy log:
```
Run python update_sub.py
[!] Lỗi: ConnectionTimeout
Traceback:
  ...
```

## Khi deploy Cloudflare Pages không ăn

**Cần:** Cloudflare Dashboard → Pages → project → Deployments → click deployment → View build log

---

## Template báo lỗi chuẩn

```
**Tính năng:** [tên tính năng]
**Triệu chứng:** [mô tả chính xác]
**Đã thử:** [rebuild map / F5 / clear cache...]
**Log/Data:**
[paste data]
```

---

## Lỗi "node link không tồn tại Zalo 0917678211"

Đây là **info node** — worker chỉ trả info nodes, không có proxy thực.
Root cause: `processB64()` trả về rỗng proxies.

**Hỏi thêm:** 
- Link Liangxin hay DJJC?
- Đã rebuild MAP chưa? Kết quả HTTP status là bao nhiêu?
- `SUB_DATA_<origToken>` trong KV có không? body_b64 bao nhiêu chars?
