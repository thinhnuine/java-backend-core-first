# CompletableFuture, Lock, Atomic Và ConcurrentHashMap

## CompletableFuture

Dùng khi cần compose async task.

```java
CompletableFuture<User> userFuture =
    CompletableFuture.supplyAsync(() -> userClient.getUser("u1"));

CompletableFuture<List<Order>> ordersFuture =
    CompletableFuture.supplyAsync(() -> orderClient.getOrders("u1"));

CompletableFuture<UserProfile> profile =
    userFuture.thenCombine(ordersFuture, UserProfile::new);
```

Method hay dùng:

- `thenApply`: transform result.
- `thenCompose`: chain async result.
- `thenCombine`: combine 2 future.
- `exceptionally`: recover từ lỗi.
- `handle`: xử lý success/failure.
- `allOf`: chờ nhiều future.

## Lock

```java
Lock lock = new ReentrantLock();
lock.lock();
try {
    // critical section
} finally {
    lock.unlock();
}
```

Ưu điểm so với `synchronized`:

- `tryLock`.
- interruptible lock.
- nhiều `Condition`.

## Atomic

```java
AtomicInteger counter = new AtomicInteger();
counter.incrementAndGet();
```

Atomic phù hợp cho state đơn giản. Với invariant nhiều field, lock thường rõ hơn.

## ConcurrentHashMap

Ưu tiên method atomic:

```java
counts.merge(key, 1, Integer::sum);
```

```java
cache.computeIfAbsent(key, ignored -> loadValue(key));
```

Tránh check-then-act tách rời.

## Deadlock

Deadlock thường xuất hiện khi:

- Thread A giữ lock 1, chờ lock 2.
- Thread B giữ lock 2, chờ lock 1.

Cách giảm:

- lock ordering cố định.
- giữ lock càng ngắn càng tốt.
- tránh gọi code ngoài khi đang giữ lock.
- dùng timeout với `tryLock` khi phù hợp.

## Bài Tập Nhanh

Viết deadlock demo với 2 lock, sau đó sửa bằng lock ordering.

Các supplyAsync không truyền executor ở ví dụ dùng common pool; bài gọi blocking I/O nên chọn executor phù hợp và có lifecycle rõ. Timeout của CompletableFuture không mặc nhiên hủy công việc I/O bên dưới; vẫn cần timeout ở HTTP/DB client. Bản đồ executor và chính sách timeout là một phần của thiết kế, không phải chỉ thêm exceptionally là xong.
