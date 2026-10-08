# Bài Tập Tuần 6

> **Chọn phạm vi:** làm phần được chỉ định trong [README module](README.md) và roadmap. Benchmark, log processor lớn, tự viết pool/queue/rate limiter, GC/reflection/proxy là bài mở rộng; checklist đầy đủ dưới đây không bắt buộc trước khi qua mốc học mới.

## Bài 1: Counter Race

Yêu cầu:

- `UnsafeCounter`.
- `SynchronizedCounter`.
- `VolatileCounter`.
- `AtomicCounter` nếu muốn preview.

Test:

- 10 threads.
- 100_000 increments mỗi thread.
- Chạy nhiều lần.

Ghi chú:

- Bản nào sai?
- Bản nào đúng?
- Vì sao `volatile` không đủ?

## Bài 2: Stop Flag

Viết worker loop:

```java
while (running) {
    // do work
}
```

Làm 2 bản:

- `running` thường.
- `volatile running`.

Quan sát và giải thích visibility.

## Bài 3: Bounded Buffer Với `wait/notifyAll`

Viết `BoundedBuffer<T>`:

- Constructor nhận capacity.
- `put(T item)` block nếu full.
- `take()` block nếu empty.
- Không nhận null item.

Yêu cầu:

- Dùng `while` khi wait.
- Dùng `notifyAll`, không dùng `notify` trước khi hiểu rõ.
- Có producer/consumer demo.

## Learning Log

Trả lời:

- Race condition tuần này nằm ở đâu?
- Monitor lock là gì?
- `volatile` giúp gì và không giúp gì?
- Tại sao `wait` phải nằm trong `while`?
- Bạn sẽ tránh shared mutable state bằng cách nào?

## Quan sát và chứng minh là hai việc khác nhau

UnsafeCounter có thể cho đúng kết quả trong một lần chạy; điều đó không chứng minh an toàn. Hãy phân tích lịch xen kẽ read–modify–write và chỉ ra quan hệ đồng bộ bảo vệ bản sửa. Đợi join của mọi worker trước khi đọc kết quả. Stop-flag demo có thể treo: chạy trong process riêng có thời gian dừng; tránh println/sleep dùng như “cách sửa visibility”. Trong lab coordination, xử lý InterruptedException rõ ràng và bảo đảm worker thoát khi kết thúc demo.
