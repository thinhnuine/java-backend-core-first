# Các Lỗi Concurrency Kinh Điển

## Race Condition

Nhiều thread đọc/ghi shared state mà không có coordination đúng.

Dấu hiệu:

- Test thỉnh thoảng fail.
- Kết quả nhỏ hơn expected.
- Log nhìn "không thể xảy ra" nhưng vẫn xảy ra.

## Deadlock

Các thread chờ lock của nhau mãi.

Cách debug:

- thread dump.
- tìm thread BLOCKED/WAITING.
- xem lock đang giữ và đang chờ.

## Visibility Bug

Thread A ghi, thread B không thấy.

Thường gặp với:

- stop flag không volatile.
- publish object chưa an toàn.
- cache state không có synchronization.

## Thread Starvation

Task không có cơ hội chạy vì thread pool bị chiếm.

Ví dụ:

- Task trong fixed pool chờ task khác cũng submit vào chính pool đó.
- Blocking call quá lâu trong pool nhỏ.

## Lock Contention

Nhiều thread tranh cùng lock, throughput giảm.

Cách giảm:

- giảm critical section.
- chia lock theo shard.
- dùng concurrent structure phù hợp.
- tránh shared mutable state.

## Bài Tập Nhanh

Tạo `concurrency-bugs.md`:

- Mỗi bug có code snippet hoặc mô tả.
- Ghi triệu chứng.
- Ghi nguyên nhân.
- Ghi cách sửa.
