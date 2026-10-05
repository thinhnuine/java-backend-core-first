# Virtual Threads Và Structured Concurrency

## Virtual Threads

Virtual threads nhẹ hơn platform threads, phù hợp cho blocking I/O nhiều request.

```java
try (var executor = Executors.newVirtualThreadPerTaskExecutor()) {
    Future<String> result = executor.submit(() -> blockingCall());
    System.out.println(result.get());
}
```

Điểm cần nhớ:

- Virtual thread không làm CPU-bound code nhanh hơn.
- Blocking I/O là use case chính.
- Vẫn cần hiểu race condition, lock, shared state.

## Platform Thread Vs Virtual Thread

Platform thread map gần với OS thread, tốn tài nguyên hơn.

Virtual thread được JVM schedule, tạo số lượng lớn dễ hơn. Code blocking đọc vẫn tự nhiên hơn so với callback/future chain trong nhiều case.

## Khi Nào Dùng Virtual Thread

Hợp:

- HTTP request handler có nhiều blocking I/O.
- Gọi DB/service ngoài kiểu blocking.
- Muốn code tuần tự dễ đọc.

Không phải thuốc tiên cho:

- CPU-bound computation.
- Shared mutable state.
- Lock contention nặng.

## Structured Concurrency

Structured concurrency giúp gom vòng đời nhiều task con vào một scope rõ ràng. Tùy Java version, API có thể là preview, nên học concept trước.

Ý tưởng:

- Task con sống trong scope cha.
- Nếu một task fail, có chính sách cancel task còn lại.
- Code async dễ đọc hơn.

## Mental Model Cho FE Dev

JS event loop giúp concurrency bằng non-blocking I/O trên một main thread.

Java có thể dùng nhiều OS/virtual threads, nên vấn đề shared memory rõ hơn. Bạn được viết blocking code dễ đọc hơn, nhưng phải quản lý thread safety.

## Bài Tập Nhanh

Viết demo:

- Tạo 1000 task sleep 100ms bằng fixed thread pool nhỏ.
- Tạo 1000 task sleep 100ms bằng virtual thread executor.
- So sánh thời gian và giải thích.
