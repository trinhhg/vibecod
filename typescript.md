# 📘 TYPESCRIPT RULES

## Types
- Phải định nghĩa rõ (explicit) kiểu trả về cho các hàm public.
- Dùng `interface` cho cấu trúc object (shapes), dùng `type` aliases cho unions/intersections.
- Ưu tiên dùng `unknown` thay vì `any` — phải thu hẹp kiểu (narrow types) trước khi sử dụng.
- Dùng const assertions (`as const`) cho các literal types.
- Sử dụng discriminated unions để quản lý trạng thái (state management).

## Generics
- Dùng generics cho các tiện ích dùng chung (reusable utilities), đừng lạm dụng cho mọi thứ.
- Đặt tên tham số generic có ý nghĩa mô tả: dùng `TItem` thay vì `T` khi có nhiều generics.
- Ràng buộc generics bằng `extends` khi có thể.

## Strict Mode
- Bật `strict: true` trong `tsconfig` — không có ngoại lệ.
- Sử dụng `noUncheckedIndexedAccess` để truy cập array/object an toàn hơn.
- KHÔNG BAO GIỜ dùng `@ts-ignore` — hãy dùng `@ts-expect-error` kèm theo lời giải thích nếu bắt buộc.

## Patterns
- Sử dụng toán tử `satisfies` để validate types mà không làm mở rộng kiểu (widening).
- Dùng `Readonly<T>` cho các dữ liệu không được phép thay đổi (mutated).
- Ưu tiên dùng `Record<K, V>` thay vì `{ [key: string]: V }`.
- Sử dụng các utility types như `Extract`, `Exclude`, `Pick`, `Omit` để biến đổi kiểu dữ liệu.

## Enums
- Ưu tiên dùng `const enums` hoặc union types thay vì enums thông thường.
- Dùng giá trị chuỗi (string values) cho các enums xuất hiện trong dữ liệu được serialize (vd: JSON).
