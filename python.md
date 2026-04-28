# 🐍 PYTHON RULES

## Style & Structure
- Sử dụng type hints cho tất cả các khai báo hàm (chữ ký hàm) và kiểu trả về.
- Ưu tiên sử dụng f-strings thay vì `.format()` hoặc định dạng `%`.
- Dùng `pathlib` thay vì `os.path` cho các thao tác với file.
- Ưu tiên list/dict/set comprehensions thay vì `map/filter` nếu nó dễ đọc hơn.
- Sử dụng `dataclasses` hoặc `Pydantic` models thay vì `dict` thuần túy cho dữ liệu có cấu trúc.

## Error Handling
- Bắt đúng loại exception cụ thể, KHÔNG BAO GIỜ dùng `except:` trống (bare except).
- Sử dụng `contextlib.suppress()` cho các exception đã dự đoán trước.
- Trả về sớm (Return early) để tránh code bị lồng (nesting) quá sâu.

## Imports
- Gom nhóm các imports theo thứ tự: Thư viện chuẩn (stdlib) → Thư viện bên thứ 3 (third-party) → Thư viện nội bộ (local). Phân tách các nhóm bằng dòng trống.
- Ưu tiên dùng import tuyệt đối (absolute imports) thay vì import tương đối.
- TUYỆT ĐỐI CẤM sử dụng wildcard imports (ví dụ: `from x import *`).

## Functions
- Giữ độ dài hàm dưới 30 dòng — nếu dài hơn, hãy tách thành các hàm helper nhỏ.
- Hạn chế sử dụng `*args` và `**kwargs`, nếu dùng phải document rõ ràng.
- Ưu tiên việc trả về giá trị (returning values) thay vì chỉnh sửa trực tiếp các tham số có thể thay đổi (mutable arguments).
- Dùng generators (dùng `yield`) cho các chuỗi dữ liệu lớn thay vì build sẵn toàn bộ vào list.

## Testing
- Sử dụng `pytest` thay vì `unittest`.
- Đặt tên test có tính mô tả cao theo format: `test_<function>_<scenario>_<expected>`.
- Sử dụng fixtures cho các setup dùng chung, dùng `parametrize` cho các test chạy nhiều case.
