# `synchronized`, `volatile` Và Java Memory Model

## `synchronized`

```java
public synchronized void increment() {
    count++;
}
```

`synchronized` làm 2 việc:

- Mutual exclusion: một thời điểm chỉ một thread vào critical section cùng monitor.
- Visibility: unlock/lock tạo happens-before.

Block form:

```java
synchronized (lock) {
    count++;
}
```

Luôn lock trên object ổn định, private nếu có thể:

```java
private final Object lock = new Object();
```

## `volatile`

`volatile` đảm bảo thread đọc thấy giá trị mới nhất theo memory model.

```java
private volatile boolean running = true;
```

Phù hợp:

- stop flag.
- config flag đơn giản.
- publish reference trong một số pattern cẩn thận.

Không phù hợp:

- `count++`.
- update nhiều field cần invariant.
- operation check-then-act.

## Java Memory Model

Java Memory Model mô tả khi nào write ở thread này visible với read ở thread khác.

Bạn không cần thuộc formal spec ngay, nhưng cần biết:

- Without happens-before, thread khác có thể không thấy write mới.
- CPU/compiler/JIT có thể reorder trong giới hạn spec.
- Code "nhìn có vẻ đúng" vẫn sai nếu thiếu synchronization.

## Happens-Before Hay Gặp

- `Thread.start()` happens-before code trong thread mới.
- Code trong thread happens-before thread khác `join()` xong.
- Unlock happens-before lock sau đó trên cùng monitor.
- Write volatile happens-before read volatile sau đó cùng field.

## `wait/notifyAll`

Pattern đúng thường dùng `while`:

```java
synchronized (lock) {
    while (queue.isEmpty()) {
        lock.wait();
    }
    item = queue.removeFirst();
}
```

Dùng `while` vì:

- spurious wakeup.
- thread khác có thể lấy mất condition trước.
- condition mới là thứ quyết định được chạy tiếp.

## Bài Tập Nhanh

Sửa `UnsafeCounter` bằng:

- `synchronized`.
- `volatile int` để chứng minh vẫn sai.
- `AtomicInteger` để preview tuần sau.

Ghi lại khác biệt bằng lời của bạn.
