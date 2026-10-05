# Deep Dive Tuần 6: Thread, Race Condition Và Memory Model

## Cách Học Tuần Này

Concurrency không học bằng cách đọc định nghĩa. Bạn phải tự tạo bug, thấy kết quả sai, rồi sửa. Nếu đến từ JavaScript, cú sốc lớn là: nhiều thread có thể thật sự chạy cùng lúc và cùng đọc/ghi memory.

Mục tiêu tuần này:

- Hiểu `start()` khác `run()`.
- Tự tạo được race condition.
- Hiểu `synchronized` bảo vệ critical section.
- Hiểu `volatile` là visibility, không phải atomicity.
- Biết vì sao `wait` nên nằm trong `while`.

## Thread Mental Model

Một process Java có thể có nhiều thread. Mỗi thread có call stack riêng, nhưng cùng thấy heap chung.

```text
Thread A stack ----\
Thread B stack ----- > shared heap objects
Thread C stack ----/
```

Bug thường đến từ nhiều thread cùng sửa object trên heap.

## `start()` Vs `run()`

```java
Thread t = new Thread(task);
t.start();
```

`start()` yêu cầu JVM tạo thread mới.

```java
t.run();
```

`run()` chỉ gọi method bình thường trong thread hiện tại.

## Race Condition

`count++` nhìn như một thao tác nhưng thật ra là 3 bước:

```text
read count
add 1
write count
```

Nếu 2 thread cùng đọc `count = 10`, cả hai cùng tính 11, rồi cùng ghi 11. Một lần increment bị mất.

## `synchronized`

`synchronized` đảm bảo chỉ một thread vào critical section cùng monitor tại một thời điểm.

```java
public synchronized void increment() {
    count++;
}
```

Nó cũng tạo visibility guarantee: thread sau khi lấy lock thấy được thay đổi của thread trước khi nhả lock.

## `volatile`

`volatile` giúp thread thấy giá trị mới nhất của field.

```java
private volatile boolean running = true;
```

Nhưng `volatile int count` vẫn không làm `count++` atomic. Nó chỉ giúp đọc/ghi một field visible hơn.

## Happens-Before

Happens-before là cách Java nói: nếu A happens-before B, thì B phải thấy effect của A.

Một vài nguồn:

- `Thread.start()`
- `Thread.join()`
- unlock rồi lock cùng monitor
- write volatile rồi read volatile cùng field

Bạn chưa cần thuộc spec. Chỉ cần biết: nếu không có happens-before, code nhìn đúng vẫn có thể sai.

## `wait/notifyAll`

`wait` phải kiểm tra condition trong `while`, không phải `if`.

```java
synchronized (lock) {
    while (queue.isEmpty()) {
        lock.wait();
    }
    return queue.removeFirst();
}
```

Vì thread có thể wake up nhưng condition vẫn chưa đúng.

## Bài Tập Theo Bước

1. Viết `UnsafeCounter`.
2. Chạy 10 thread increment.
3. Ghi lại expected và actual.
4. Sửa bằng `synchronized`.
5. Thử `volatile` và giải thích vì sao vẫn sai.
6. Viết bounded buffer bằng `wait/notifyAll`.

## Lỗi Thường Gặp

- Gọi `run()` thay vì `start()`.
- Nghĩ `volatile` thay được lock.
- Lock trên object public hoặc thay đổi được.
- Dùng `if` khi `wait`.
- Test chạy pass một lần rồi tưởng concurrent code đúng.

## Câu Hỏi Tự Kiểm Tra

- Shared mutable state trong bài của bạn nằm ở đâu?
- Operation nào không atomic?
- Lock object là object nào?
- `volatile` giải quyết visibility hay atomicity?
- Nếu thread bị stuck, bạn sẽ nhìn gì đầu tiên?
