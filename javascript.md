# 🟨 JAVASCRIPT RULES

## Modern Syntax
- Dùng `const` làm mặc định, chỉ dùng `let` khi cần gán lại giá trị, TUYỆT ĐỐI KHÔNG DÙNG `var`.
- Sử dụng arrow functions cho callbacks và các hàm ngắn.
- Dùng template literals thay vì nối chuỗi (string concatenation).
- Destructure objects và arrays ngay tại nơi sử dụng.
- Ưu tiên dùng optional chaining (`?.`) và nullish coalescing (`??`) thay vì tự check thủ công.

## Async
- Dùng `async/await` thay vì dùng chuỗi `.then()`.
- Luôn xử lý lỗi bằng `try/catch` bên trong các hàm async.
- Dùng `Promise.all()` cho các tác vụ độc lập chạy song song.
- Không bao giờ mix (trộn lẫn) callbacks và promises trong cùng một luồng code.

## Functions
- Giữ cho hàm pure (thuần khiết) nhất có thể — cùng input thì luôn ra cùng output.
- Sử dụng default parameters thay vì tự check và gán giá trị mặc định thủ công.
- Trả về sớm (Return early) để giảm độ sâu nesting.
- Ưu tiên dùng named exports để hỗ trợ tree-shaking tốt hơn.

## Arrays & Objects
- Ưu tiên dùng `map/filter/reduce` thay vì vòng lặp `for` truyền thống khi biến đổi dữ liệu.
- Dùng `structuredClone()` hoặc spread operator (`...`) để copy nông (shallow copies).
- Dùng `Object.entries()`, `Object.keys()`, `Object.values()` thay vì vòng lặp `for...in`.

## Error Handling
- Throw `Error` objects, không throw string.
- Tạo các class Error custom cho những lỗi đặc thù của dự án (domain-specific errors).
- Phải luôn handle promise rejections — không bao giờ để tình trạng unhandled promise.
