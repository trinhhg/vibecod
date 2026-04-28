# 🎨 TAILWIND CSS RULES (DÀNH CHO FRONTEND)

Khi viết code giao diện (UI), BẮT BUỘC tuân thủ tư duy của một chuyên gia Tailwind CSS:

## 1. QUY TẮC SỬ DỤNG UTILITY
- **Utility-first:** Ưu tiên dùng class tiện ích. Chỉ tách thành component khi một cụm code lặp lại từ 3 lần trở lên.
- **Mobile-first:** Luôn code responsive theo hướng từ thiết bị di động lên màn hình lớn (`sm:`, `md:`, `lg:`). Tuyệt đối KHÔNG dùng media query dạng `max-width`.
- **Thứ tự class logic:** Nhóm class theo đúng thứ tự: Bố cục (Layout) → Khoảng cách (Spacing) → Typography → Màu sắc (Colors) → Hiệu ứng (Effects).
- **Hạn chế Arbitrary values:** Chỉ dùng giá trị bọc trong ngoặc vuông (VD: `[24px]`) khi bộ Design Tokens mặc định thực sự không đáp ứng được.

## 2. DESIGN TOKENS & THEME
- **Cấu hình tập trung:** Nếu có một giá trị tùy biến dùng lặp đi lặp lại, PHẢI đưa nó vào phần `extend` trong `tailwind.config.js`.
- **Màu ngữ nghĩa (Semantic colors):** Gọi màu theo chức năng như `primary`, `surface`, `muted`. KHÔNG code cứng kiểu `blue-500` rải rác khắp nơi.
- **Tôn trọng Scale:** Giữ nguyên hệ thống spacing scale mặc định của Tailwind. Chỉ mở rộng thêm, không ghi đè (replace) chúng.
- **Typography:** Luôn khai báo font size đi kèm với line-height tương ứng (VD: `text-lg/7`).

## 3. CẤU TRÚC COMPONENT
- **Dùng `@apply` có chừng mực:** Chỉ dùng `@apply` trong file CSS cho các component tĩnh, thực sự lặp lại nhiều lần (như `btn`, `input`, `card`).
- **Ưu tiên Component hóa:** Đối với UI phức tạp, hãy tách thành các Component thực thụ (React/Vue/HTML functions) thay vì lạm dụng `@apply`.
- **CẤM TUYỆT ĐỐI:** Không bao giờ dùng `@apply` cho các state variants (`hover:`, `focus:`) hoặc responsive variants. Phải giữ chúng ở HTML markup.
- Quản lý các biến thể (variants) UI bằng Class Variance Authority (CVA) hoặc các thư viện tương đương.

## 4. GIAO DIỆN TỐI (DARK MODE)
- Sử dụng variant `dark:` kết hợp với chiến lược `class` (class strategy) để hỗ trợ tính năng cho phép user tự chuyển đổi (manual toggle).
- Định nghĩa các biến màu Dark Mode ngay trong file config theme, hạn chế việc nhồi nhét override `dark:` quá dài trong markup HTML.
- Khi viết code, ngầm tự kiểm tra (Mental check) cả 2 chế độ. Dark mode cần sự hài hòa, không đơn giản chỉ là "đảo ngược màu sắc".

## 5. BỐ CỤC (LAYOUT)
- Dùng **Flexbox** cho bố cục 1 chiều (1D) và **Grid** cho bố cục 2 chiều (2D). Tailwind hỗ trợ xử lý cả hai cực kỳ dễ dàng.
- Bọc nội dung bằng class `container` kết hợp `mx-auto` và padding responsive. Không được dùng `max-width` một cách tùy tiện.
- Dùng `gap-*` để tạo khoảng cách giữa các phần tử con trong Flex/Grid. KHÔNG dùng `margin`.
- Tuyệt đối tránh dùng `absolute` positioning nếu Flex hoặc Grid có thể giải quyết được bố cục đó.

## 6. TỐI ƯU HIỆU SUẤT (PERFORMANCE)
- Mặc dù Tailwind có PurgeCSS/JIT dọn rác, nhưng **CẤM** tạo class động bằng cách nối chuỗi nội suy (string concatenation).
- **Phải viết đầy đủ tên class:** Viết `bg-red-500`, KHÔNG ĐƯỢC viết `bg-${color}-500`.
- Khai báo `safelist` một cách cực kỳ dè dẻn, chỉ dành cho những class thực sự cần sinh ra động (dynamic usage).
- Ưu tiên sử dụng các plugins chuẩn của Tailwind thay vì tự viết ra một đống custom CSS.
