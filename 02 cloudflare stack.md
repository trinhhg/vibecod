# CLOUDFLARE + GITHUB STACK — Quy tắc kỹ thuật

## Stack dự án

- **Cloudflare Pages** — host static files (HTML/CSS/JS)
- **Cloudflare Pages Functions** → `_worker.js` — xử lý API, KV
- **Cloudflare KV** — lưu trữ data (links, orders, accounts...)
- **GitHub** — version control, deploy trigger
- **GitHub Actions** — cron job chạy `update_sub.py` mỗi 60 phút

---

## Worker (_worker.js) — Constraints bắt buộc

### KHÔNG dùng:
- `require()`, `fs`, `path`, Node.js modules
- `console.log` nhiều (tốn CPU)
- Vòng lặp `await` tuần tự cho nhiều KV calls

### PHẢI dùng:
- `fetch()` native
- `env.VPN_KV.get/put/delete/list` cho KV
- `ctx.waitUntil()` cho async tasks không cần block response
- `Promise.all()` khi cần nhiều KV reads song song

### Pattern KV đúng:
```javascript
// ❌ SAI - tuần tự chậm
for (const tok of tokens) {
  const data = await env.KV.get(tok);
}

// ✅ ĐÚNG - song song nhanh
const results = await Promise.all(tokens.map(tok => env.KV.get(tok)));
```

### CORS — phải có trong MỌI response:
```javascript
const CORS = {
  "Access-Control-Allow-Origin": "*",
  "Access-Control-Allow-Methods": "GET,POST,DELETE,OPTIONS",
  "Access-Control-Allow-Headers": "Content-Type",
};
// Xử lý OPTIONS preflight
if (request.method === "OPTIONS") return new Response(null, { headers: CORS });
```

### Export pattern cho Pages Functions:
```javascript
export default {
  async fetch(request, env, ctx) {
    // handler
  }
};
```

---

## KV Storage — Quy ước naming

```
ALL_LINKS          → JSON array các link sub
ALL_ORDERS         → JSON array đơn hàng
ALL_ACCOUNTS       → JSON array tài khoản VPN nguồn
MAP_<token>        → origToken hoặc "DEAD_NODE"
ORIG_URL_<token>   → URL gốc
SUB_DATA_<token>   → {body_b64, body_yaml, info, traffic}
LOCK_<token>       → "1" nếu đang khóa
DEAD_LINKS         → JSON array links đã ngắt
ADMIN_DEVICES      → JSON array thiết bị admin
ALL_CMS            → JSON object hướng dẫn theo platform
```

TTL cho dead MAP: `{ expirationTtl: 604800 }` (7 ngày)

---

## GitHub Actions — Cấu trúc secret

Secrets trong `.github/workflows/` dùng `${{ secrets.XYZ }}`.
Secrets riêng cho Cloudflare Worker phải set TẠI Cloudflare dashboard:
- `Settings → Environment Variables` (Pages)
- Không đồng bộ tự động từ GitHub → phải set thủ công cả 2 chỗ

---

## Deploy flow

```
Push to GitHub main
  → Cloudflare Pages auto-deploy (HTML + _worker.js)
  → GitHub Actions cron: update_sub.py fetch + push KV
```

---

## isLiangxin detection

```javascript
// Detect dựa trên origUrl trong KV, không dựa trên pathname /v1 /v2
const origUrl = await env.VPN_KV.get("ORIG_URL_" + origToken);
const isLiangxin = origUrl?.includes("liangxin");
// DJJC có nhiều subdomain: djjc.cfd, www.djjc.cfd, dash.djjc.cfd, gates.djjc.cfd
// Tất cả đều chứa "djjc.cfd" → !isLiangxin
```
