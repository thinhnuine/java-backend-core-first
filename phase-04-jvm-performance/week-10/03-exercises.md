# Bài Tập Tuần 10

Đây là đề để tự làm, không phải đáp án. Chọn từng mục theo README module; không cần làm hết trong một buổi. Tự gõ code, dùng AI để xin gợi ý sau khi đã thử.

## Bài 1: Heap Dump Analysis

Heap dump là ảnh object trong bộ nhớ; analysis là tìm thứ giữ dữ liệu. **Tự làm trước:** vẽ một list giữ dữ liệu không còn cần. **Gợi ý:** tìm nơi xóa reference. Tạo dump là phần mở rộng cuối file.

## Bài 2: Mini Component Scanner

Scanner đọc danh sách class và chọn class có annotation.

- Tạo dấu MyComponent có thể đọc lúc chạy.
- Chuẩn bị ba class, chỉ hai class có dấu.
- Lọc danh sách `Class<?>` có sẵn.

**Gợi ý:** Không quét cả package trước khi lọc danh sách đơn giản đúng.

**Đạt khi:** Chọn đúng hai class, bỏ class không đánh dấu. Tự gõ phần triển khai, không dùng AI sinh lời giải.

## Bài 3: Dynamic Proxy Logger

Dynamic proxy là object trung gian nhận rồi chuyển lời gọi. **Tự làm trước:** vẽ proxy ghi log quanh Calculator. **Gợi ý:** phải giữ kết quả và lỗi gốc của target (object thật).

## Bài 4: Spring Magic Note

Ghi Spring biết class nào cần quản lý bằng cách nào và lời gọi nào đi qua proxy. **Tự làm:** so gọi từ ngoài với tự gọi trong cùng object. **Gợi ý:** annotation không tự thay mọi lời gọi.

## Đọc thêm sau

Tạo heap dump trên process bài tập còn chạy, xác định chuỗi reference giữ object. Proxy đầy đủ ghi tên method, tham số và thời gian; không nuốt lỗi gốc. Không cần thực hiện hai lab này để bắt đầu Spring.
