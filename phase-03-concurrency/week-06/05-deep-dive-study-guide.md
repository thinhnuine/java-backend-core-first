# Mentor Guide Tuần 6: Thread, Race Condition Và Memory Model

## 1. Ý chính

Java có thể chạy nhiều thread thật sự cùng lúc, và các thread có thể cùng đọc/ghi object trên heap. Vì vậy bạn phải học `race condition`, `synchronized`, `volatile` và Java Memory Model. Đây là phần rất khác với JavaScript frontend vì JS thường chạy logic app trên một main thread.

## 2. Giải thích code

Ví dụ race condition:

```java
package dev.thinh.javacore;

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

Giải thích:

- `private int count`: shared mutable state.
- `increment()`: tăng count.
- `count++`: nhìn như một thao tác nhưng thật ra gồm read, add, write.

Chạy nhiều thread:

```java
UnsafeCounter counter = new UnsafeCounter();

Thread t1 = new Thread(() -> {
    for (int i = 0; i < 100_000; i++) {
        counter.increment();
    }
});

Thread t2 = new Thread(() -> {
    for (int i = 0; i < 100_000; i++) {
        counter.increment();
    }
});

t1.start();
t2.start();
t1.join();
t2.join();

System.out.println(counter.count());
```

Bạn kỳ vọng `200000`, nhưng có thể nhỏ hơn.

Sửa bằng `synchronized`:

```java
public synchronized void increment() {
    count++;
}
```

`synchronized` khóa method để mỗi thời điểm chỉ một thread vào method đó trên cùng object.

## 3. Vì sao thiết kế như vậy

`count++` không atomic:

```text
Thread A read count = 10
Thread B read count = 10
Thread A write 11
Thread B write 11
```

Hai lần increment nhưng kết quả chỉ tăng một.

`volatile` không sửa được bug này:

```java
private volatile int count;
```

`volatile` giúp visibility, tức thread thấy giá trị mới hơn, nhưng `count++` vẫn gồm nhiều bước. Muốn atomic, dùng `synchronized`, `Lock`, hoặc `AtomicInteger`.

## 4. Liên hệ với Frontend

Trong JS frontend, bạn hay lo async race kiểu request A về sau request B. Nhưng code JS của bạn thường vẫn chạy trên một thread chính. Java backend có race ở memory thật: hai thread có thể cùng sửa một field tại cùng thời điểm.

React state update cũng dạy bạn tránh dựa vào state cũ không an toàn:

```javascript
setCount(prev => prev + 1)
```

Trong Java multi-thread, vấn đề còn sâu hơn vì có shared memory và visibility.

## 5. Khi nào dùng và không dùng

| Công cụ | Khi dùng | Khi tránh |
| --- | --- | --- |
| `synchronized` | Critical section nhỏ, rule đơn giản | Lock giữ quá lâu |
| `volatile` | Flag stop/start, visibility đơn giản | Compound operation như `count++` |
| `AtomicInteger` | Counter/state đơn giản | Invariant nhiều field |
| `wait/notifyAll` | Học coordination căn bản | Code production phức tạp nếu có library tốt hơn |

## 6. Bẫy hay gặp

- Gọi `run()` thay vì `start()`.
- Nghĩ `volatile` làm `count++` atomic.
- Dùng `if` thay `while` khi `wait`.
- Lock trên object public.
- Test pass vài lần rồi tưởng concurrency code đúng.

## 7. Thuật ngữ mới

- `thread`: luồng thực thi.
- `race condition`: kết quả phụ thuộc timing giữa thread.
- `critical section`: đoạn code cần bảo vệ.
- `visibility`: thread này có thấy write của thread khác không.
- `atomicity`: thao tác không bị chen ngang giữa chừng.
- `happens-before`: quan hệ đảm bảo visibility trong Java Memory Model.

## 8. Bài tập nhỏ để tự gõ lại

1. Viết `UnsafeCounter`.
2. Chạy 2 thread, mỗi thread increment 100k lần.
3. Sửa bằng `synchronized`.
4. Thử đổi sang `volatile int` và chứng minh vẫn sai.
5. Viết learning log: bug đến từ atomicity hay visibility?

Học tiếp theo: chỉ khi bạn tự thấy counter sai, phần `synchronized` mới thật sự "ngấm".
