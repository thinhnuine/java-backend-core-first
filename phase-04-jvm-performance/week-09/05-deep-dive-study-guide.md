# Deep Dive Tuần 9: JVM Memory, Class Loading, JIT Và GC

## Cách Học Tuần Này

Tuần này học cách nhìn Java như một runtime, không chỉ là ngôn ngữ. Bạn không cần thành performance engineer ngay. Bạn cần biết khi app chậm, ăn RAM, GC nhiều, hoặc lỗi classpath thì nên nhìn theo hướng nào.

## Heap, Stack, Metaspace

Heap chứa object.

Stack chứa frame của từng method call theo từng thread.

Metaspace chứa metadata class.

Nói đơn giản:

```text
object sống lâu -> nghĩ tới heap
call stack sâu -> nghĩ tới stack
class loading nhiều -> nghĩ tới metaspace
```

## StackOverflowError

Thường do recursion quá sâu:

```java
void call() {
    call();
}
```

Mỗi lần gọi method thêm frame vào stack. Quá sâu thì tràn stack.

## OutOfMemoryError

Không phải lúc nào cũng giống nhau:

- Java heap space
- Metaspace
- Direct buffer memory
- Unable to create native thread

Thông điệp lỗi cho bạn biết vùng nào có vấn đề.

## Class Loading

Classpath là nơi JVM tìm class. Maven dependency cuối cùng cũng trở thành classpath lúc chạy.

Hai lỗi dễ gặp:

- `ClassNotFoundException`: runtime tìm class theo tên nhưng không có.
- `NoClassDefFoundError`: compile từng thấy class, nhưng runtime không load được.

## JIT

JIT tối ưu code nóng khi app chạy.

Vì vậy benchmark tự viết dễ sai:

- chưa warm-up
- input quá nhỏ
- JVM optimize mất code
- đo một lần rồi kết luận

Nếu thật sự microbenchmark, dùng JMH. Ở giai đoạn này, chỉ cần biết đừng overclaim.

## Garbage Collector

GC thu hồi object không còn reachable từ GC roots.

Leak trong Java thường là object vẫn reachable nhưng không còn cần nữa.

Ví dụ:

- static list giữ object
- cache không eviction
- listener không unsubscribe

## GC Log

Chạy với:

```bash
java -Xlog:gc* -jar app.jar
```

Đọc các ý:

- GC xảy ra thường không?
- pause time bao lâu?
- heap before/after thế nào?
- có Full GC không?

## Bài Tập Theo Bước

1. Tạo object ngắn hạn nhiều.
2. Chạy với heap nhỏ.
3. Bật GC log.
4. Tạo static list giữ object.
5. So sánh log.
6. Viết note cẩn trọng, không kết luận quá tay.

## Lỗi Thường Gặp

- Nghĩ GC log đọc một dòng là biết hết.
- Benchmark không warm-up.
- Nhầm memory leak với memory usage cao hợp lệ.
- Không phân biệt heap OOM và metaspace OOM.

## Câu Hỏi Tự Kiểm Tra

- Object thường nằm ở đâu?
- Stack overflow khác heap OOM thế nào?
- Classpath lỗi biểu hiện ra sao?
- JIT ảnh hưởng benchmark thế nào?
- Object leak trong Java nghĩa là gì?
