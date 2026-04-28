# 🛡️ BẢO MẬT (SECURITY RULES)

## Input Validation & Data Protection
- Validate TẤT CẢ input của user — kiểu dữ liệu, độ dài, định dạng, khoảng giá trị.
- Sử dụng Allowlists (danh sách cho phép) thay vì Denylists.
- Không bao giờ chỉ tin tưởng vào validation phía Client-side.
- Mã hóa dữ liệu nhạy cảm (TLS). Tuyệt đối không log passwords, tokens, hoặc PII (Thông tin cá nhân).
- Sử dụng Biến môi trường (Environment variables) cho secrets.

## Authentication & Authorization
- Sử dụng bcrypt/argon2 để hash mật khẩu (tuyệt đối không dùng MD5/SHA).
- Implement rate limiting cho các endpoint xác thực.
- Kiểm tra quyền (Permissions) trên MỌI request ở backend, không chỉ ẩn UI ở frontend.
- Cấu hình mặc định là đóng — từ chối truy cập nếu không được cấp quyền rõ ràng (Fail closed / Deny by default).
