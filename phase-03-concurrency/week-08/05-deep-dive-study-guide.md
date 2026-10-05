# Deep Dive Tuần 8: Virtual Threads Và Concurrency Capstone

## Cách Học Tuần Này

Tuần này không phải để thuộc API mới nhất. Mục tiêu là hiểu virtual thread giải quyết pain nào, và hoàn thành ba bài tổng hợp concurrency: thread pool, producer-consumer, rate limiter.

## Platform Thread Vs Virtual Thread

Platform thread gần với OS thread. Tạo quá nhiều sẽ tốn tài nguyên.

Virtual thread nhẹ hơn, do JVM quản lý. Bạn có thể tạo rất nhiều virtual thread cho blocking I/O.

Nhưng virtual thread không biến shared mutable state thành an toàn. Race condition vẫn là race condition.

## Use Case Tốt Cho Virtual Thread

Hợp:

- request handler gọi DB/service ngoài blocking
- nhiều task chờ I/O
- muốn code tuần tự dễ đọc

Không hợp để kỳ vọng:

- CPU-bound nhanh hơn
- lock contention biến mất
- code shared state tự đúng

## Structured Concurrency

Ý tưởng: task con có scope rõ ràng với task cha. Nếu một task fail, scope có thể cancel task còn lại. Code async trở nên có cấu trúc hơn.

Bạn chỉ cần hiểu concept, vì API có thể thay đổi theo Java version.

## Thread Pool Tự Viết

Thread pool đơn giản gồm:

- task queue
- worker threads
- submit method
- shutdown flag

Câu hỏi quan trọng:

- Shutdown xong có nhận task mới không?
- Task đã submit trước shutdown có chạy hết không?
- Task throw exception thì worker có chết không?
- Queue có bounded không?

## Producer-Consumer

Producer thêm item, consumer lấy item.

Nếu queue đầy, producer chờ.

Nếu queue rỗng, consumer chờ.

Pattern này giúp bạn hiểu blocking coordination rất tốt.

## Rate Limiter

Rate limiter trả lời: request này có được phép qua không?

Thuật toán:

- Fixed window: dễ, nhưng burst ở ranh giới window.
- Sliding window: chính xác hơn, phức tạp hơn.
- Token bucket: cân bằng, cho burst nhỏ.

## Bài Tập Theo Bước

1. Viết API trước, chưa implement.
2. Viết test đơn luồng.
3. Implement bản đơn giản.
4. Thêm test nhiều thread.
5. Ghi bug gặp được.
6. Nhờ AI review race/deadlock.

## Lỗi Thường Gặp

- Shutdown flag không volatile/không lock.
- Worker chết khi task throw exception.
- Producer-consumer dùng `if` thay `while`.
- Rate limiter test chỉ đơn luồng.
- Tin concurrent test pass là chứng minh tuyệt đối.

## Câu Hỏi Tự Kiểm Tra

- Virtual thread giúp gì và không giúp gì?
- Thread pool xử lý exception trong task thế nào?
- Shutdown behavior được định nghĩa rõ chưa?
- Rate limiter dùng thuật toán nào?
- Bug concurrency khó nhất bạn gặp là gì?
