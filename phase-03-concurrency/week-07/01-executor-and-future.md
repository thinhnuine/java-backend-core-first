# ExecutorService Và Future

## Vì Sao Không Tự Tạo Thread Mãi

Tự `new Thread` cho mỗi task dễ gây:

- quá nhiều thread.
- khó shutdown.
- khó quản lý queue.
- khó handle exception.

`ExecutorService` tách task submission khỏi thread management.

## Fixed Thread Pool

```java
ExecutorService executor = Executors.newFixedThreadPool(4);
try {
    Future<Integer> future = executor.submit(() -> 42);
    Integer value = future.get();
} finally {
    executor.shutdown();
}
```

Luôn nghĩ tới shutdown. Thread pool bị quên shutdown có thể làm app không dừng.

## `Future`

`Future` đại diện kết quả sẽ có trong tương lai.

Method hay dùng:

- `get()`: block chờ kết quả.
- `get(timeout, unit)`: block có timeout.
- `cancel`.
- `isDone`.
- `isCancelled`.

## Shutdown Đúng

Pattern:

```java
executor.shutdown();
if (!executor.awaitTermination(10, TimeUnit.SECONDS)) {
    executor.shutdownNow();
}
```

Trong bài tập nhỏ có thể đơn giản hơn, nhưng phải biết vấn đề.

## Exception Trong Task

Exception từ `Callable` sẽ được wrap trong `ExecutionException` khi gọi `future.get()`.

```java
try {
    future.get();
} catch (ExecutionException e) {
    Throwable cause = e.getCause();
}
```

Đừng nuốt cause.

## Bài Tập Nhanh

Viết `TaskRunner`:

- Nhận list `Callable<T>`.
- Chạy bằng fixed thread pool.
- Trả list kết quả theo thứ tự input.
- Nếu task lỗi, giữ cause trong custom result type.
