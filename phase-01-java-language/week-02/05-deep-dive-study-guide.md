# Deep Dive Tuần 2: Generics, Enum Và Nested Class

## Cách Học Tuần Này

Tuần 2 bắt đầu có cảm giác "Java rất Java": generics dài, wildcard khó đọc, enum mạnh hơn bạn tưởng, nested class thì nhìn hơi lạ. Đừng cố thuộc cú pháp ngay. Hãy học theo hướng: mỗi cú pháp giải quyết vấn đề gì, và khi nào không nên dùng.

Mục tiêu cuối tuần:

- Đọc được chữ ký generic mà không hoảng.
- Viết được generic class/method nhỏ.
- Hiểu `? extends` và `? super` bằng ví dụ đọc/ghi.
- Biết type erasure giới hạn bạn ở runtime.
- Dùng enum để gom behavior, không chỉ thay string constant.

## Mental Model Cho Generics

Generics là cách nói với compiler: "class/method này làm việc với một type nào đó, nhưng tôi muốn type đó vẫn an toàn".

Không generic:

```java
Object value = box.get();
String text = (String) value;
```

Bạn phải cast, lỗi có thể nổ runtime.

Generic:

```java
Box<String> box = new Box<>();
String text = box.get();
```

Compiler biết `box` chứa `String`, nên bạn ít lỗi hơn.

## Generic Class Vs Generic Method

Generic class dùng khi cả object xoay quanh type đó:

```java
public final class Repository<ID, T> {
}
```

Generic method dùng khi chỉ một method cần type linh hoạt:

```java
public static <T> T first(List<T> values) {
    return values.get(0);
}
```

Câu hỏi tự hỏi:

- Type parameter này có cần tồn tại ở cấp class không?
- Hay chỉ một method cần nó?

## PECS Nói Chậm

PECS: Producer Extends, Consumer Super.

Nếu collection là nguồn để bạn đọc ra `T`, dùng `extends`:

```java
public double total(List<? extends Number> numbers) {
    double sum = 0;
    for (Number number : numbers) {
        sum += number.doubleValue();
    }
    return sum;
}
```

Bạn có thể truyền `List<Integer>`, `List<Long>`, `List<Double>`. Nhưng bạn không nên add item vào đó, vì compiler không biết list thật sự là list của subtype nào.

Nếu collection là nơi bạn ghi `T` vào, dùng `super`:

```java
public void addDefaults(List<? super Integer> values) {
    values.add(1);
    values.add(2);
}
```

Bạn có thể truyền `List<Integer>`, `List<Number>`, hoặc `List<Object>`.

## Type Erasure

Generics trong Java chủ yếu bảo vệ ở compile time. Runtime không giữ đủ thông tin generic.

Vì vậy không thể:

```java
// if (value instanceof List<String>) {}
// T item = new T();
```

Khi cần biết type ở runtime, thường truyền thêm:

```java
Class<T> type
```

Điều này sẽ gặp lại khi học JSON parser, reflection, JPA, Jackson.

## Enum Nên Học Kỹ

Trong Java, enum không chỉ là constant. Nó có thể có field, constructor, method.

Ví dụ tốt:

```java
public enum TaskStatus {
    TODO,
    IN_PROGRESS,
    DONE;

    public boolean isTerminal() {
        return this == DONE;
    }
}
```

Enum giúp bạn tránh string rải rác:

```java
if (status.equals("done")) {}
```

String dễ typo, khó refactor, khó biết toàn bộ giá trị hợp lệ.

## Nested Class

Static nested class hợp khi class phụ chỉ có nghĩa trong context class ngoài.

Ví dụ:

```java
public final class ApiResponse<T> {
    public static final class Error {
    }
}
```

Inner class ít dùng hơn vì nó giữ reference tới outer object. Nếu không cần reference đó, ưu tiên static nested class.

## Bài Tập Theo Bước

1. Viết `Repository<ID, T>` interface.
2. Implement `InMemoryRepository<ID, T>` bằng `HashMap`.
3. Thêm validation null cho id/value.
4. Viết `WorkflowStatus` enum.
5. Viết test cho `canMoveTo`.
6. Viết `LruCache<K, V>`, trước tiên dùng `LinkedHashMap`.
7. Nếu còn sức, tự viết LRU bằng `HashMap + doubly linked list`.

## Lỗi Thường Gặp

- Đặt tên generic quá mơ hồ khi API phức tạp.
- Dùng wildcard dù generic method đơn giản hơn.
- Dùng `Optional.get()` bừa bãi.
- Enum chỉ chứa constant nhưng logic transition lại nằm rải rác ở service.
- Dùng inner class trong khi static nested class đủ.

## Câu Hỏi Tự Kiểm Tra

- Khi nào dùng generic class?
- Khi nào dùng generic method?
- Vì sao `List<Integer>` không phải subtype trực tiếp của `List<Number>`?
- `? extends Number` đọc được gì và ghi được gì?
- Type erasure khiến bạn không làm được gì ở runtime?
- Enum Java hơn string constant ở điểm nào?
- Nested class có giúp API rõ hơn không, hay làm code khó đọc hơn?
