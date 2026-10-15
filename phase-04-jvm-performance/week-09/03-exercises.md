# Bài Tập Tuần 9

Đây là đề để tự làm, không phải đáp án. Chọn từng mục theo README module; không cần làm hết trong một buổi. Tự gõ code, dùng AI để xin gợi ý sau khi đã thử.

## Bài 1: Memory Regions Demo

Nhìn vai trò vùng nhớ trước khi tạo lỗi.

- Vẽ method gọi method và object được giữ trong list.
- Tạo/xóa một phần tử.
- Ghi reference nào còn tồn tại.

**Gợi ý:** Không cần cố làm hết bộ nhớ ở lượt đầu.

**Đạt khi:** Phân biệt stack lời gọi với dữ liệu object. Tự gõ phần triển khai, không dùng AI sinh lời giải.

## Bài 2: GC Log Lab

GC log ghi hoạt động thu gom bộ nhớ.

- Chạy ứng dụng nhỏ có bật log.
- Ghi tên collector và một thông tin đọc được.

**Gợi ý:** Quote tùy chọn như java '-Xlog:gc*' Main.java trong zsh.

**Đạt khi:** Không nhầm log ít với bộ nhớ không có vấn đề. Tự gõ phần triển khai, không dùng AI sinh lời giải.

## Bài 3: Benchmark Warm-Up

Warm-up là giai đoạn chạy trước phép đo chính.

- Đo cùng phép tính nhiều lần.
- Giữ input và sử dụng kết quả.
- Ghi sự biến động.

**Gợi ý:** Đừng kết luận mọi thay đổi đều do JIT.

**Đạt khi:** Báo cáo nêu rõ giới hạn phép đo. Tự gõ phần triển khai, không dùng AI sinh lời giải.

## Bài 4: Class Loading Note

Classpath là nơi Java tìm class khi chạy. **Tự làm:** chạy file từ sai thư mục rồi đọc lỗi, sau đó sửa đường dẫn. **Gợi ý:** ghi lệnh và thư mục hiện tại trước khi đoán dependency bị lỗi.

## Đọc thêm sau

StackOverflowError do đệ quy quá sâu, thử nghiệm allocation lớn, giới hạn heap và lỗi class loading chỉ chạy có kiểm soát trong lab riêng. Benchmark nghiêm túc dùng JMH.
