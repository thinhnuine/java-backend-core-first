# Bài Tập Tuần 7

Đây là đề để tự làm, không phải đáp án. Chọn từng mục theo README module; không cần làm hết trong một buổi. Tự gõ code, dùng AI để xin gợi ý sau khi đã thử.

## Bài 1: Async User Profile

Async profile ghép hai kết quả độc lập có thể đến sau.

- Tạo fake client trả user và order.
- Chạy hai việc bằng executor rõ vòng đời.
- Ghép kết quả, thử một nhánh lỗi.

**Gợi ý:** Fake là implementation đơn giản không gọi mạng thật.

**Đạt khi:** Thành công và lỗi đều có cách xử lý; executor đóng được. Tự gõ phần triển khai, không dùng AI sinh lời giải.

## Bài 2: Concurrent Word Counter

Đếm từ nhiều luồng trên dữ liệu nhỏ.

- Tạo bản một luồng làm mốc.
- Chia dòng thành task.
- Gộp count theo khóa bằng merge.

**Gợi ý:** Đợi mọi task rồi so với mốc.

**Đạt khi:** Từ lặp được đếm đủ và giải thích được thao tác atomic. Tự gõ phần triển khai, không dùng AI sinh lời giải.

## Bài 3: Deadlock Lab

Deadlock lab là bài các luồng chờ khóa lẫn nhau. **Tự làm:** vẽ hai khóa bị lấy ngược thứ tự, rồi sửa thứ tự chung. **Gợi ý:** mô hình giấy trước; nếu chạy demo treo thì process riêng và có cách dừng.

## Bài 4: Atomic Counter

So cách bảo vệ một bộ đếm.

- Làm synchronized và AtomicInteger.
- Dùng cùng số lần tăng.
- So độ dễ giải thích trước tốc độ.

**Gợi ý:** AtomicInteger gộp phép tăng; không tách get/set.

**Đạt khi:** Hai bản đúng và nêu được dữ liệu được bảo vệ. Tự gõ phần triển khai, không dùng AI sinh lời giải.

## Đọc thêm sau

LongAdder, timeout ở client thật và thử nghiệm tải cao sau. Timeout Future không tự hủy công việc I/O bên dưới.
