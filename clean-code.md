# 🧹 THỰC HÀNH CODE SẠCH (CLEAN CODE RULES)

## Đặt tên (Naming)
- Sử dụng tên mang tính mô tả, thể hiện rõ ý định.
- Tránh viết tắt trừ khi nó quá phổ biến (URL, ID, HTTP).
- Đặt tên biến Boolean như một câu hỏi: `isActive`, `hasPermission`, `canEdit`.
- Đặt tên hàm như một hành động: `fetchUser`, `calculateTotal`, `validateInput`.

## Hàm (Functions)
- Chỉ làm MỘT việc duy nhất — nếu mày có thể viết thành một đoạn văn để mô tả hàm, nghĩa là nó đang làm quá nhiều việc.
- Giữ số lượng tham số từ 3 trở xuống — nếu nhiều hơn, hãy gom thành một object.
- Trả về sớm (Return early / Guard clauses) để tránh code lồng nhau (nesting) quá sâu.
- Tránh side effects trong các hàm có trả về giá trị.

## Cấu trúc & Sự đơn giản (Structure & Simplicity)
- Giữ file dưới 300 dòng — tách ra nếu lớn hơn. Nhóm các code liên quan lại gần nhau (co-location).
- Ưu tiên giải pháp đơn giản, nhàm chán thay vì cố tỏ ra thông minh.
- Đừng trừu tượng hóa (abstract) quá sớm — hãy đợi đến khi code lặp lại lần thứ 3.
- Xóa sạch dead code (code không bao giờ chạy tới). 
- Tối ưu hóa cho sự dễ đọc trước, chỉ tối ưu hiệu năng khi thực sự có vấn đề được đo lường.

## Comments
- Viết code tự giải thích — Comment là để giải thích TẠI SAO (WHY), không phải LÀM GÌ (WHAT).
- Sử dụng comment cho: business rules, các luồng xử lý tạm (workarounds), hoặc các quyết định logic khó hiểu.
- XÓA NGAY code bị comment-out — lịch sử đã có Git lo.
