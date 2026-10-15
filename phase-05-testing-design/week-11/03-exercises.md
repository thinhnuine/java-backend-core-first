# Bài Tập Tuần 11

Đây là đề để tự làm, không phải đáp án. Chọn từng mục theo README module; không cần làm hết trong một buổi. Tự gõ code, dùng AI để xin gợi ý sau khi đã thử.

## Bài 1: Test LRU Cache

LRU cache chỉ kiểm nếu bạn đã làm bài cache. **Tự làm:** với capacity 2, ghi chuỗi put/get để xác định mục bị loại. **Gợi ý:** get có thể đổi thứ tự gần đây; chưa có cache thì test TaskService trước.

## Bài 2: Test Event Emitter

Event emitter gọi các listener (bên nghe sự kiện). **Tự làm:** kiểm đăng ký, gọi, gỡ đăng ký. **Gợi ý:** quy định một listener lỗi có dừng các listener sau không trước khi viết test.

## Bài 3: Parameterized Validation

Parameterized validation dùng nhiều input cho cùng quy tắc kiểm.

- Chọn title của Task.
- Liệt kê null, trắng, hợp lệ, quá 100 ký tự.
- Tạo test cho từng case.

**Gợi ý:** Lượt đầu viết test riêng; gộp parameterized sau.

**Đạt khi:** Expected rõ cho từng input. Tự gõ phần triển khai, không dùng AI sinh lời giải.

## Bài 4: Mock/Fake Comparison

So fake tự viết với mock được cấu hình bằng thư viện.

- Chọn service nhận Sender.
- Fake lưu message đã gửi.
- Nếu đã học Mockito, viết kiểm tra tương đương.

**Gợi ý:** Không cần làm cả hai khi fake đã đủ rõ.

**Đạt khi:** Nêu cách nào dễ đọc hơn trong đúng tình huống này. Tự gõ phần triển khai, không dùng AI sinh lời giải.

## Learning Log

Ghi test nào bắt được lỗi khi bạn cố ý làm hỏng code. **Gợi ý:** một test kiểm input/output có giá trị hơn nhiều test chỉ gọi method mà không kiểm kết quả.

## Đọc thêm sau

Chỉ mở rộng sau khi bài nhỏ chạy đúng. Ghi một lỗi đã gặp và cách bạn tự tìm nguyên nhân.
