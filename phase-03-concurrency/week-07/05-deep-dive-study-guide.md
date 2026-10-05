# Deep Dive Tuần 7: Executor, CompletableFuture, Lock Và Atomic

## Cách Học Tuần Này

Tuần 6 bạn tự quản lý thread. Tuần 7 bạn học abstraction production hơn. Trong code backend thật, bạn hiếm khi `new Thread` lung tung. Bạn dùng executor, future, lock, atomic class và concurrent collections.

## ExecutorService

Executor tách "task cần chạy" khỏi "thread nào chạy".

```java
ExecutorService executor = Executors.newFixedThreadPool(4);
```

Bạn submit task:

```java
Future<Integer> result = executor.submit(() -> 42);
```

Và nhớ shutdown:

```java
executor.shutdown();
```

Thread pool không shutdown có thể làm app không dừng.

## Future

`Future` là lời hứa: sau này sẽ có kết quả hoặc lỗi.

```java
Integer value = future.get();
```

`get()` block thread hiện tại. Dùng quá nhiều blocking có thể làm mất lợi ích async.

## CompletableFuture

`CompletableFuture` giúp compose task.

```java
userFuture.thenCombine(orderFuture, UserProfile::new);
```

Method hay gặp:

- `thenApply`: transform value.
- `thenCompose`: chain future.
- `thenCombine`: combine hai future.
- `exceptionally`: recover khi lỗi.
- `handle`: xử lý cả success/failure.

## Lock

`ReentrantLock` giống lock explicit.

```java
lock.lock();
try {
    // critical section
} finally {
    lock.unlock();
}
```

Luôn unlock trong `finally`.

## Atomic

Atomic class hợp cho state đơn giản:

```java
AtomicInteger count = new AtomicInteger();
count.incrementAndGet();
```

Nếu state gồm nhiều field phải giữ invariant cùng nhau, lock thường rõ hơn.

## ConcurrentHashMap

Đừng viết check-then-act:

```java
if (!map.containsKey(key)) {
    map.put(key, value);
}
```

Dùng operation atomic:

```java
map.computeIfAbsent(key, this::load);
map.merge(key, 1, Integer::sum);
```

## Deadlock

Deadlock thường là hai thread giữ lock và chờ nhau.

Giảm bằng:

- lock ordering
- timeout với `tryLock`
- không giữ lock khi gọi code ngoài
- critical section nhỏ

## Bài Tập Theo Bước

1. Viết service gọi fake user và fake order song song.
2. Combine bằng `thenCombine`.
3. Thêm error handling.
4. Viết word counter bằng `ConcurrentHashMap.merge`.
5. Tạo deadlock demo.
6. Sửa bằng lock ordering.

## Lỗi Thường Gặp

- Quên shutdown executor.
- Dùng common pool mà không hiểu workload.
- Gọi `get()` quá sớm làm mất parallelism.
- Lock rồi quên unlock khi exception.
- Dùng `ConcurrentHashMap` nhưng update không atomic.

## Câu Hỏi Tự Kiểm Tra

- Task của bạn CPU-bound hay I/O-bound?
- Executor size chọn dựa trên gì?
- CompletableFuture pipeline có block sớm không?
- Exception async được xử lý ở đâu?
- Lock ordering của bạn là gì?
