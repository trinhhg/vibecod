# ADMIN.HTML — Patterns và Data Structures

## Data structures trong localStorage/API

```javascript
// linksDb — mảng link sub
[{
  id: "LNK-1234567890",
  orig: "https://liangxin.xyz/...?OwO=abc",  // link gốc
  masked: "https://vpntrinhhg.pages.dev/v1?OwO=abc#VPNTrinhHg", // link ẩn
  email: "khach@gmail.com",
  expDate: "30/05/2026",    // DD/MM/YYYY
  linkIndex: "2",           // số máy/thiết bị
  date: "21:30 1/5/2026"
}]

// accountsDb — tài khoản VPN nguồn + email khách
[{
  email: "admin@vpn.com",
  username: "user123",
  password: "pass456",
  note: "Liangxin Premium",
  customers: [
    { email: "khach1@gmail.com", note: "PC văn phòng" },
    { email: "khach2@gmail.com", note: "" }
  ]
}]

// deadLinksDb — khi deadDate là ISO string để filter 24h chính xác
[{
  ...link_fields,
  deadDate: "2026-05-01T14:30:00.000Z"  // ISO format để so sánh
}]
```

## API calls pattern

```javascript
// GET + cache localStorage fallback
async function apG(path, def) {
  try {
    const r = await fetch(API + path + "?_=" + Date.now());
    if (!r.ok) throw 0;
    const d = await r.json();
    localStorage.setItem("c" + path, JSON.stringify(d));
    return d;
  } catch {
    try { return JSON.parse(localStorage.getItem("c" + path)) || def; }
    catch { return def; }
  }
}

// POST — fire and forget với localStorage backup
async function apP(path, data) {
  localStorage.setItem("c" + path, JSON.stringify(data));
  try { await fetch(API + path, { method: "POST", headers: {"Content-Type":"application/json"}, body: JSON.stringify(data) }); }
  catch {}
}
```

## Lock state persistence — fix F5 mất khóa

```javascript
// Sau khi fetchAll, sync lock state từ KV
async function syncLockCache() {
  const tokens = linksDb.map(l => getT(l.masked)).filter(Boolean);
  await Promise.all(tokens.map(async tok => {
    try {
      const r = await fetch(API + "/api/lock?token=" + tok);
      if (r.ok) { const d = await r.json(); lockCache[tok] = !!d.locked; }
    } catch {}
  }));
}
```

## Tree view links — nhóm theo link gốc

```javascript
// Nhóm links theo orig URL
const grps = {};
for (const l of linksDb) {
  if (!grps[l.orig]) grps[l.orig] = { orig: l.orig, items: [] };
  grps[l.orig].items.push(l);
}

// Sắp xếp theo linkIndex tăng dần (1,2,3...)
const sorted = [...g.items].sort((a,b) =>
  (parseInt(a.linkIndex)||1) - (parseInt(b.linkIndex)||1)
);
```

## Domain detection khi tạo link

```javascript
// Liangxin → /v1 + ?OwO=token
// DJJC (bất kỳ subdomain djjc.cfd) → /v2 + ?token=token
if (orig.includes("liangxin"))     masked = PD + "/v1" + qs + "#VPNTrinhHg";
else if (orig.includes("djjc.cfd")) masked = PD + "/v2" + qs + "#VPNTrinhHg";
else return toast("Domain không hỗ trợ!", "err");
```

## Toast notifications

```javascript
// type: "ok" (green), "warn" (amber), "err" (red)
function toast(msg, type = "ok") { ... }
```

## Full screen layout

```css
html, body { height: 100%; margin: 0; }
main { width: 100%; padding: 1rem; }
/* KHÔNG dùng max-w-xxx trên main */
```
