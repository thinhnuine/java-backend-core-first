# Mentor Guide Tuần 7: Executor, CompletableFuture, Lock Và Atomic

> **Chọn phạm vi:** làm phần được chỉ định trong [README module](README.md) và roadmap. Benchmark, log processor lớn, tự viết pool/queue/rate limiter, GC/reflection/proxy là bài mở rộng; checklist đầy đủ dưới đây không bắt buộc trước khi qua mốc học mới.

## 1. Ý chính

Tuần này bạn chuyển từ tự tạo thread sang dùng abstraction thực tế hơn. `ExecutorService` quản lý thread pool, `CompletableFuture` giúp compose async task, `Lock` cho control rõ hơn `synchronized`, còn `Atomic*` xử lý state đơn giản theo cách thread-safe.

## 2. Giải thích code

Ví dụ Executor:

```java
ExecutorService executor = Executors.newFixedThreadPool(4);
try {
    Future<Integer> future = executor.submit(() -> 42);
    Integer value = future.get();
    System.out.println(value);
} finally {
    executor.shutdown();
}
```

Giải thích:

- `Executors.newFixedThreadPool(4)`: tạo pool có 4 worker thread.
- `submit(() -> 42)`: gửi task vào pool.
- `Future<Integer>`: đại diện kết quả sẽ có sau.
- `future.get()`: block chờ kết quả.
- `finally`: đảm bảo shutdown kể cả khi có lỗi.
- `executor.shutdown()`: không nhận task mới và cho pool dừng dần.

Ví dụ Atomic:

```java
AtomicInteger count = new AtomicInteger();
count.incrementAndGet();
```

`incrementAndGet` là atomic operation, an toàn hơn `count++` trong multi-thread.

## 3. Vì sao thiết kế như vậy

Tự `new Thread` liên tục dễ tạo quá nhiều thread:

```java
for (Task task : tasks) {
    new Thread(() -> process(task)).start();
}
```

Nếu có 10k task, bạn có thể tạo 10k thread và làm hệ thống nghẹt. Fixed pool giới hạn số worker, nhưng queue mặc định không giới hạn. Muốn giới hạn backlog phải cấu hình bounded queue và chính sách khi quá tải.

Với `CompletableFuture`, nếu bạn gọi `get()` quá sớm:

```java
User user = userFuture.get();
List<Order> orders = orderFuture.get();
```

Nếu cả hai future đã submit trước đoạn get, chúng vẫn có thể chạy song song; get chỉ chặn caller. Mất song song khi chờ user xong rồi mới submit orders. thenCombine giúp compose kết quả mà không chặn caller tại đây; nó không tự tạo song song nếu trước đó công việc chưa được lên lịch đúng.

## 4. Liên hệ với Frontend

`CompletableFuture` có cảm giác hơi giống `Promise`:

```javascript
Promise.all([fetchUser(), fetchOrders()])
```

Java:

```java
userFuture.thenCombine(ordersFuture, UserProfile::new)
```

Khác biệt: Java còn phải nghĩ tới executor/thread pool nào chạy task. JavaScript Promise thường chạy trên event loop/runtime abstraction.

## 5. Khi nào dùng và không dùng

| Công cụ | Khi dùng | Khi tránh |
| --- | --- | --- |
| `ExecutorService` | Chạy nhiều task có kiểm soát | Quên shutdown |
| `Future` | Cần kết quả task đơn giản | Compose phức tạp |
| `CompletableFuture` | Combine async task | Pipeline khó đọc, block sớm |
| `Lock` | Cần `tryLock`, timeout, condition | `synchronized` đủ đơn giản |
| `AtomicInteger` | Counter đơn giản | State nhiều field cần invariant |
| `ConcurrentHashMap` | Nhiều thread update map | Logic nhiều bước không atomic |

## 6. Bẫy hay gặp

- Quên `executor.shutdown()`.
- Dùng common pool cho blocking I/O nặng.
- Gọi `future.get()` quá sớm.
- `lock.lock()` nhưng quên `unlock()` trong `finally`.
- Dùng `ConcurrentHashMap` nhưng vẫn check-then-act sai.

## 7. Thuật ngữ mới

- `thread pool`: nhóm thread tái sử dụng để chạy task.
- `Future`: handle cho kết quả tương lai.
- `blocking`: thread dừng chờ kết quả.
- `compose`: ghép nhiều async operation.
- `atomic operation`: thao tác không bị chen ngang.
- `deadlock`: các thread chờ nhau mãi.

## 8. Bài tập nhỏ để tự gõ lại

1. Viết `AsyncUserProfileService`.
2. Fake `UserClient` sleep 200ms.
3. Fake `OrderClient` sleep 200ms.
4. Chạy tuần tự rồi chạy song song bằng `CompletableFuture`.
5. So sánh thời gian.
6. Thêm `exceptionally` để handle lỗi.

Học tiếp theo: sau khi dùng executor, luôn tự hỏi "pool này shutdown ở đâu?".
