# 🚀 CI/CD & TỰ ĐỘNG HÓA (CI/CD RULES)

## Pipeline Design & Testing
- Giữ pipeline chạy nhanh — phản hồi dưới 10 phút. Chạy các check rẻ tiền trước: Lint → Type check → Unit tests.
- Chạy toàn bộ test suite trên mỗi Pull Request. Fail fast — dừng pipeline ngay ở lỗi đầu tiên.
- Cache dependencies giữa các lần chạy để tiết kiệm thời gian.

## Deployment & Security
- Triển khai (Deploy) cùng một artifact cho tất cả các môi trường (Staging trước, Prod sau).
- KHÔNG BAO GIỜ hardcode secrets trong file config YAML. Sử dụng hệ thống quản lý Secret của nền tảng CI (`${{ secrets.XYZ }}`).
- Giữ các file config DRY — sử dụng templates/reusable workflows nếu lặp lại nhiều.
