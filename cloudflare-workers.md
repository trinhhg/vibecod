# ☁️ CLOUDFLARE WORKERS (CORE RULES)

Mày là một chuyên gia về hệ sinh thái Cloudflare Workers. TUYỆT ĐỐI tuân thủ:

## Xử lý Request (Request Handling)
- Export một default object chứa `fetch` handler, không dùng `addEventListener`.
- Phải khai báo type cho interface `Env` cho tất cả bindings (KV, D1, R2, secrets).
- LUÔN LUÔN trả về object `Response`, không bao giờ được throw error mà không bắt lại (unhandled errors).
- Sử dụng `Request.clone()` trước khi đọc body nếu mày cần đọc nó nhiều lần.
- Dùng URL pattern matching hoặc router nhẹ (như Hono), cấm dùng các chuỗi `if/else` khổng lồ.

## Môi trường & Bindings (KV, D1, R2)
- Lấy secret qua `env.SECRET_NAME`. Khai báo mọi bindings trong `wrangler.toml`.
- **KV Storage:** Nhớ kỹ KV là eventual consistency (nhất quán cuối) - KHÔNG dùng cho dữ liệu cần đọc ngay lập tức sau khi ghi. LUÔN LUÔN set TTL (`expirationTtl`) cho dữ liệu dạng cache. Không có hàm `getMany`, phải batch bằng cách dùng `list` + gọi `get` cho từng item.
- **D1 Database:** Luôn dùng parameter queries với dấu `?`, tuyệt đối cấm nối chuỗi SQL (string concatenation). D1 dựa trên SQLite, hãy dùng type của SQLite.
- **R2 Storage:** Dùng cho file lớn. KV có giới hạn 25MB nên cấm nhét file to vào KV. Luôn set `Content-Type` khi put objects.

## Hiệu năng & Lỗi ngớ ngẩn (Performance & Pitfalls)
- Workers có giới hạn RAM 128MB. Cấm parse JSON khổng lồ, hãy dùng stream khi có thể.
- Sử dụng `ctx.waitUntil()` cho việc ghi log, analytics, và các tác vụ async không quan trọng sau khi đã trả response cho user.
- **Node.js Modules:** Không có fs, path... trừ khi dùng cờ `nodejs_compat`.
- `Date.now()` trả về thời gian lúc request BẮT ĐẦU, không phải thời gian thực tế hiện tại đang chạy.
- LUÔN LUÔN xử lý CORS preflight (OPTIONS requests) một cách rõ ràng.
