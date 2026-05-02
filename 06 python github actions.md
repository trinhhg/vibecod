# PYTHON + GITHUB ACTIONS — update_sub.py

## Mục đích file update_sub.py

Chạy mỗi 60 phút qua GitHub Actions:
1. Fetch `ALL_LINKS` từ Worker API → lấy link gốc
2. Fetch từng link gốc (dùng UA `v2rayN/6.23`) → nhận base64 hoặc YAML
3. Decode → rename nodes → build base64 mới + YAML đầy đủ
4. Push lên KV qua `POST /api/push_data`

## Detect Liangxin vs DJJC trong Python

```python
is_liangxin = "liangxin" in orig_url
# DJJC: tất cả subdomain của djjc.cfd → not is_liangxin
# www.djjc.cfd, djjc.cfd, dash.djjc.cfd, gates.djjc.cfd → đều ok
```

## Xử lý response Liangxin

Liangxin server **đôi khi** trả YAML thẳng thay vì base64 (server-side UA filtering).
Cần detect và xử lý cả 2 trường hợp:

```python
raw = r.text.strip()
# Detect YAML raw
if raw.startswith("mixed-port:") or "proxies:" in raw[:200]:
    # Parse YAML trực tiếp
    parsed_yaml = yaml.safe_load(raw)
    proxies = parsed_yaml.get("proxies", [])
else:
    # Decode base64 bình thường
    decoded = base64.b64decode(pad(raw)).decode("utf-8", errors="ignore")
    # Parse URI lines
```

## Cấu trúc LIANGXIN_RULES vs DJJC_RULES

Hai bộ rules riêng biệt — KHÔNG gộp chung:
- `LIANGXIN_RULES`: có thêm `IP-CIDR,1.1.1.1/32`, `IP-CIDR,8.8.8.8/32`, `DOMAIN,cn.bing.com,DIRECT`
- `DJJC_RULES`: không có 3 entries trên

```python
rules = LIANGXIN_RULES if is_liangxin else DJJC_RULES
```

## Error handling chuẩn

```python
try:
    r = requests.get(url, headers={"User-Agent": "v2rayN/6.23"}, timeout=30)
    if r.status_code != 200:
        print(f"  [!] HTTP {r.status_code}")
        continue
    # process...
except Exception as e:
    import traceback
    print(f"  [!] Lỗi: {e}")
    traceback.print_exc()
    continue
```

## Log format cho GitHub Actions

```python
print("=== VPN Trinh Hg — update_sub.py ===")
print(f"\n→ [Liangxin] token: {token[:12]}...")
print(f"  b64 length: {len(raw)} chars")
print(f"  Parsed {len(proxies)} proxies")
print(f"  [OK] Push → HTTP {r.status_code}")
```

## Rename dict — khi node chưa có trong dict

```python
new_name = rename_map.get(old_name)
if new_name is None:
    # Fallback: giữ tên gốc + suffix
    new_name = old_name + " - VPN Trinh Hg" if "VPN Trinh Hg" not in old_name else old_name
    print(f"  [WARN] No rename rule for: {repr(old_name)}")
    # Log này giúp phát hiện nodes mới chưa được map
```
