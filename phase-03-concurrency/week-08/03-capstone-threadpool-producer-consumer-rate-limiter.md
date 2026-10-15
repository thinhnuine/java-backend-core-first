# Capstone Concurrency: Thread Pool, Producer-Consumer, Rate Limiter

Đây là đề để tự làm, không phải đáp án. Chọn từng mục theo README module; không cần làm hết trong một buổi. Tự gõ code, dùng AI để xin gợi ý sau khi đã thử.

## Bài 1: Simple Thread Pool

Thread pool là nhóm worker lấy task từ hàng đợi. Bài tự viết chỉ làm sau khi dùng được ExecutorService; yêu cầu sâu ở cuối file. **Tự làm trước:** dùng pool có sẵn chạy ba task. **Gợi ý:** kiểm cả lúc task lỗi và lúc đóng.

## Bài 2: Producer-Consumer

Producer tạo dữ liệu, consumer lấy dữ liệu ra xử lý.

- Dùng hàng đợi có giới hạn.
- Một producer thêm, một consumer lấy.
- Quy định lúc dừng.

**Gợi ý:** Thử ArrayBlockingQueue có sẵn trước khi tự viết chờ/đánh thức.

**Đạt khi:** Không mất item, không chờ mãi sau khi hết việc. Tự gõ phần triển khai, không dùng AI sinh lời giải.

## Bài 3: Rate Limiter

Rate limiter giới hạn số thao tác trong khoảng thời gian. **Tự làm trước:** viết quy tắc “tối đa 3 lần trong 1 giây” bằng ví dụ thời điểm. **Gợi ý:** phân biệt cửa sổ cố định với cửa sổ trượt; chưa cần code nhiều luồng.

## Deliverable

Deliverable là phần nộp để kiểm tra: code, cách chạy, một test lỗi và ghi chú về dừng việc. **Tự làm:** ghi một tình huống còn chưa xử lý. **Gợi ý:** đừng mô tả bản lab như công cụ production.

## Đọc thêm sau

Pool tự viết: từ chối task mới sau shutdown, xử lý hết việc đã nhận theo chính sách, task lỗi không làm mất toàn bộ worker. Buffer tự viết cần while và xử lý interruption. Rate limiter có fixed/sliding window và token bucket (ngân sách được bổ sung dần); kiểm ranh giới thời gian và nhiều luồng, dùng đồng hồ có thể điều khiển trong test.
