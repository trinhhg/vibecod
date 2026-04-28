# 📝 BẢO TỒN COMMENT (PRESERVE COMMENTS RULES)

LUÔN LUÔN giữ nguyên tất cả các code comment hiện có trong quá trình refactor, dọn dẹp, tối ưu hóa hoặc bất kỳ thay đổi mã nguồn nào khác.

## 1. CÁC LOẠI COMMENT ĐƯỢC BẢO VỆ (TUYỆT ĐỐI KHÔNG XÓA HAY SỬA)
- **Tham chiếu hệ thống/Task:** JIRA, Linear, GitHub Issues, Asana task IDs.
- **Cảnh báo an toàn:** Các cụm từ như "do not modify", "breaks if changed", "required by", "deprecated" (đặc biệt là các comment ghi chú bằng tiếng Việt).
- **Tham chiếu tuân thủ/Pháp lý:** SOC2, GDPR, HIPAA, các yêu cầu quy định, ghi chú audit.
- **Ngữ cảnh có mốc thời gian:** Lịch trình chuyển đổi (migration), ngày khai tử hàm (deprecation dates), ngày review.
- **Giải thích Business Logic:** Giải thích TẠI SAO (why) code lại hoạt động theo cách đó, chứ không chỉ mô tả LÀM GÌ (what).

## 2. COMMENT CẤU TRÚC (STRUCTURAL COMMENTS)
- Các comment giải thích luồng code (code flow), quy trình từng bước, hoặc các hành vi không rõ ràng (non-obvious behavior) cũng phải được giữ lại.
- **Nguyên tắc vàng:** Nếu một comment giải thích *tại sao (why)* thay vì *cái gì (what)*, thì BẮT BUỘC phải giữ nó lại.

## 3. KHI NÀO MỚI ĐƯỢC PHÉP XÓA COMMENT?
CHỈ được phép xóa comment nếu chúng rơi vào các trường hợp sau:
- **Sai rành rành:** Mâu thuẫn rõ ràng với đoạn code hiện tại mà nó đang mô tả.
- **Trùng lặp 100%:** Là bản sao chép y hệt của một comment khác trong cùng một file.
- **Boilerplate tự động sinh:** Những comment sinh ra tự động từ tool nhưng không mang lại thông tin gì có ích (ví dụ: `// default constructor`).

**🚨 QUY TẮC CHỐT:** Khi phân vân không biết có nên xóa hay không, **HÃY GIỮ LẠI COMMENT ĐÓ.**
