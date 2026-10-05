# Thread Và Race Condition

## Thread Cơ Bản

```java
Thread thread = new Thread(() -> {
    System.out.println(Thread.currentThread().getName());
});

thread.start();
thread.join();
```

`start()` tạo thread mới. Gọi `run()` trực tiếp chỉ là method call bình thường trên thread hiện tại.

## Thread Lifecycle Tối Giản

Các trạng thái hay gặp:

- NEW: object thread mới tạo, chưa start.
- RUNNABLE: có thể đang chạy hoặc chờ CPU.
- BLOCKED: chờ monitor lock.
- WAITING/TIMED_WAITING: chờ signal hoặc timeout.
- TERMINATED: đã kết thúc.

## Race Condition

Race condition xảy ra khi kết quả phụ thuộc timing giữa các thread.

Ví dụ:

```java
count++;
```

Dòng này không atomic. Nó gồm:

1. Read `count`.
2. Add 1.
3. Write lại `count`.

Hai thread có thể cùng đọc giá trị cũ và ghi đè nhau.

## Demo `UnsafeCounter`

```java
public class UnsafeCounter {
    private int count;

    public void increment() {
        count++;
    }

    public int count() {
        return count;
    }
}
```

Chạy nhiều thread increment, kết quả thường nhỏ hơn kỳ vọng.

## Shared Mutable State

Concurrency khó nhất khi có shared mutable state.

Cách giảm rủi ro:

- Tránh share state nếu có thể.
- Dùng immutable object.
- Dùng local variable.
- Bảo vệ shared state bằng lock/atomic/concurrent structure.
- Giữ critical section nhỏ.

## Bài Tập Nhanh

Viết `CounterRaceDemo`:

- 10 thread.
- Mỗi thread increment 100_000 lần.
- In expected và actual.
- Chạy 10 lần, ghi lại lần nào sai.
