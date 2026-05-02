# HƯỚNG DẪN NẠP VÀO GEMINI GEM

## Thứ tự nạp files vào Gem (quan trọng — nạp theo thứ tự này)

1. `01-gemini-workflow.md` — Quy trình làm việc cốt lõi
2. `02-cloudflare-stack.md` — Kỹ thuật Cloudflare/GitHub
3. `03-code-quality.md` — Quy tắc code sạch (đã fix mâu thuẫn comment)
4. `04-bug-diagnosis.md` — Chuẩn đoán bug không cần terminal
5. `05-security-deploy.md` — Bảo mật và deploy
6. `06-python-github-actions.md` — update_sub.py patterns
7. `07-admin-html-patterns.md` — admin.html data structures
8. `08-how-to-report-bugs.md` — Cách user báo lỗi chuẩn

## Instruction cho Gem (paste vào System Instruction)

```
Bạn là AI assistant chuyên vibe coding web trên Cloudflare Pages + GitHub.
Stack: Cloudflare Pages, Workers, KV, GitHub Actions, Tailwind CSS, vanilla JS, Python.

LUẬT CỨNG:
1. Luôn trả TOÀN BỘ file khi fix code — KHÔNG snippet
2. Mental Dry-Run trước khi viết: trace luồng logic từ đầu đến cuối
3. Không có terminal → hỏi user cung cấp F12 log / KV data khi cần
4. Hỏi tối đa 1 câu/lần khi thiếu thông tin
5. Thêm version comment đầu file: // v1.x - Mô tả fix - date

FILES DỰ ÁN:
- index.html: trang khách (public)
- admin.html: trang quản trị
- _worker.js: Cloudflare Pages Worker (API + KV)
- update_sub.py: GitHub Actions cron script
```

## Các files đã loại bỏ khỏi bộ quy tắc

- `typescript.md` — Dự án không dùng TypeScript, chỉ vanilla JS
- `tailwind-css.md` — Đã tích hợp vào 03-code-quality.md (CDN only, không build)
- `preserve-comments.md` + `clean-code.md` — Đã merge thành rule rõ ràng trong 03-code-quality.md (loại bỏ mâu thuẫn)
- `ci-cd.md` — Đã tích hợp vào 05-security-deploy.md và 06-python-github-actions.md
- `security.md` — Đã tích hợp vào 05-security-deploy.md
- `javascript.md` — Đã tích hợp vào các file liên quan

## So sánh với bộ cũ (10 files → 8 files)

| File cũ | Vấn đề | Xử lý |
|---|---|---|
| clean-code.md | Mâu thuẫn với preserve-comments | Merge → 03 với rule rõ ràng |
| preserve-comments.md | Mâu thuẫn với clean-code | Merge → 03 |
| typescript.md | Không dùng TS trong dự án | Loại bỏ |
| tailwind-css.md | Quá chung chung | Tích hợp vào 03 |
| ci-cd.md | Thiếu Cloudflare sync | Tích hợp + fix vào 05, 06 |
| security.md | Chung chung | Tích hợp vào 05 |
| javascript.md | Quá chung | Tích hợp vào các file |
| cloudflare-workers.md | Thiếu Promise.all pattern | Fix + đưa vào 02 |
