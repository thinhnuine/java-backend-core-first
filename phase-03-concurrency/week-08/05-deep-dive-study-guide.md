# Mentor Guide Tuần 8: Virtual Threads Và Concurrency Capstone

> **Chọn phạm vi:** làm phần được chỉ định trong [README module](README.md) và roadmap. Benchmark, log processor lớn, tự viết pool/queue/rate limiter, GC/reflection/proxy là bài mở rộng; checklist đầy đủ dưới đây không bắt buộc trước khi qua mốc học mới.

## 1. Ý chính

`Virtual threads` là feature Java 21 giúp chạy rất nhiều task blocking I/O với chi phí thread thấp hơn platform thread. Nó làm code blocking dễ viết hơn, nhưng không làm shared mutable state tự an toàn. Tuần này bạn cũng làm capstone concurrency: thread pool, producer-consumer và rate limiter.

## 2. Giải thích code

Ví dụ virtual thread executor:

```java
try (var executor = Executors.newVirtualThreadPerTaskExecutor()) {
    Future<String> future = executor.submit(() -> {
        Thread.sleep(100);
        return "done";
    });

    System.out.println(future.get());
}
```

Giải thích:

- `var`: Java tự suy luận type local variable.
- `newVirtualThreadPerTaskExecutor()`: mỗi task chạy trên một virtual thread.
- `try (...)`: executor được close tự động.
- `Thread.sleep(100)`: mô phỏng blocking I/O.
- `future.get()`: chờ kết quả.

Producer-consumer mental model:

```text
producer -> bounded queue -> consumer
```

Nếu queue đầy, producer chờ. Nếu queue rỗng, consumer chờ.

## 3. Vì sao thiết kế như vậy

Platform thread tốn tài nguyên hơn. Nếu mỗi request blocking DB/API cần một platform thread, hệ thống có thể bị giới hạn bởi số thread. Virtual thread giúp bạn giữ style code tuần tự mà vẫn scale tốt hơn cho blocking I/O.

Nhưng virtual thread không sửa bug này:

```java
count++;
```

Nếu 1000 virtual threads cùng gọi `count++`, race condition vẫn tồn tại. Virtual thread giải quyết chi phí thread, không giải quyết correctness của shared state.

## 4. Liên hệ với Frontend

Frontend JS dùng event loop và async/await:

```javascript
const user = await fetchUser()
```

Virtual thread cho phép Java backend viết code blocking nhìn cũng tuần tự:

```java
User user = userClient.getUser();
```

Nhưng runtime bên dưới khác nhau. JS async không tạo hàng nghìn OS thread, còn Java virtual thread do JVM schedule.

## 5. Khi nào dùng và không dùng

| Công cụ | Khi dùng | Khi tránh |
| --- | --- | --- |
| Virtual thread | Blocking I/O nhiều, code tuần tự | CPU-bound task |
| Platform thread pool | Control worker cố định | Tạo quá nhiều thread |
| Producer-consumer | Điều phối tốc độ producer/consumer | Logic đơn giản không cần queue |
| Rate limiter | Giới hạn request/action | Không định nghĩa rõ window/rule |
| Token bucket | Cho phép burst nhỏ | Muốn rule cực đơn giản ban đầu |

## 6. Bẫy hay gặp

- Nghĩ virtual thread làm CPU-bound nhanh hơn.
- Dùng virtual thread để né học lock/thread safety.
- Thread pool tự viết không shutdown rõ.
- Producer-consumer dùng `if` thay `while`.
- Rate limiter chỉ test single-thread.

## 7. Thuật ngữ mới

- `virtual thread`: thread nhẹ do JVM quản lý.
- `platform thread`: thread gần với OS thread.
- `blocking I/O`: operation chờ network/file/DB.
- `bounded queue`: queue có capacity giới hạn.
- `rate limiter`: cơ chế giới hạn tần suất request.
- `token bucket`: thuật toán rate limit dùng token refill.

## 8. Bài tập nhỏ để tự gõ lại

1. Tạo demo 1000 task sleep 100ms bằng fixed pool 10 thread.
2. Tạo demo 1000 task sleep 100ms bằng virtual thread executor.
3. Ghi thời gian chạy.
4. Viết `BoundedQueue<T>`.
5. Viết rate limiter fixed window.

Học tiếp theo: khi test concurrent code, chạy test nhiều lần và cố tình tạo contention.
