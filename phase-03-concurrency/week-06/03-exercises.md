# Bài Tập Tuần 6

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
