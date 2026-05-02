# CHUẨN ĐOÁN VÀ SỬA BUG — Không terminal, không run code

## Nguyên tắc: Diagnose từ triệu chứng

Vì không chạy được lệnh, phải suy luận từ mô tả. Dùng bảng này:

| Triệu chứng | Root cause thường gặp | Hỏi thêm gì |
|---|---|---|
| "Hiện node link không tồn tại" | b64 decode fail → chỉ có info nodes | Server trả YAML hay base64? Thử UA khác? |
| "F5 mất trạng thái" | State trong JS variable, không persist | Có đang dùng localStorage/KV fetch chưa? |
| "Rebuild xong vẫn như cũ" | Cache KV còn cũ, chưa delete | body_b64 trong KV còn > 2000 chars? |
| "Failed to fetch" | URL sai / CORS / timeout | Copy chính xác URL + status code từ Network tab |
| "Deploy xong không thấy thay đổi" | Pages cache / sai branch / build fail | GitHub Actions log có đỏ không? |
| "Rename node không ăn" | Cache YAML cũ / dict key sai emoji | Tên node thực tế trong app là gì (copy paste)? |
| "Lock 1 tiếng vẫn chưa khóa" | App Clash cache local, re-fetch theo interval | Đây là behavior đúng. Lock hiệu lực ở lần fetch KẾ TIẾP |

## Pattern trace bug Worker

```
Request vào Worker:
  1. Parse URL, method
  2. Check auth nếu cần
  3. MAP_reqToken → origToken (DEAD_NODE?)
  4. LOCK_reqToken → trả LOCKED?
  5. SUB_DATA_origToken → cache đủ? trả cache
  6. Fetch origin → processB64() → save KV async
  7. Trả kết quả

Bug "link không tồn tại":
  → Bước 6: processB64 trả rỗng?
  → Nguyên nhân: Server trả YAML (không phải b64) → decB64 fail → chỉ info nodes
  → Fix: Detect rawB64.includes("proxies:") → parse YAML trực tiếp
```

## Detect Liangxin vs DJJC

```javascript
// ĐÚNG: dùng origUrl từ KV (persist qua mọi lần fetch)
const origUrl = await env.KV.get("ORIG_URL_" + origToken);
const isLiangxin = origUrl?.includes("liangxin"); // liangxin.xyz
// DJJC: djjc.cfd (bất kỳ subdomain: www, dash, gates...)

// SAI: dùng pathname /v1 /v2 (chỉ đúng khi tạo link, không reliable)
```

## Debug KV data

Khi user paste KV value, kiểm tra:
- `SUB_DATA_xxx.body_b64.length` < 2000 → cache ngắn → chỉ info nodes → cần fetch lại
- `MAP_xxx` = "DEAD_NODE" → link đã chết
- `LOCK_xxx` = "1" → đang khóa
- `ORIG_URL_xxx` có đúng domain không (liangxin vs djjc)

## Dead links auto-expire

Logic chuẩn: Khi GET `/api/deadlinks`, Worker filter bỏ items có `deadDate` > 24h:

```javascript
const MS_24H = 24 * 60 * 60 * 1000;
const alive = all.filter(item => {
  if (!item.deadDate) return true;
  const parsed = new Date(item.deadDate).getTime();
  return isNaN(parsed) || (Date.now() - parsed) < MS_24H;
});
if (alive.length < all.length) await env.KV.put("DEAD_LINKS", JSON.stringify(alive));
```
