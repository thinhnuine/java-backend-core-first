# Tra Cứu Sau: Generics Và Class Lồng Nhau

Chỉ đọc sau khi đã qua checklist cơ bản. Đây là các snippet tham khảo từ bài cũ; cần bổ sung import/class/caller tương ứng, không chạy nguyên từng block như chương trình độc lập.

## 2. Generic Method

```java
public final class Lists {
    private Lists() {
    }

    public static <T> T first(List<T> values) {
        if (values == null || values.isEmpty()) {
            throw new IllegalArgumentException("values must not be empty");
        }
        return values.get(0);
    }
}
```

## 3. Bounded Type

```java
public static <T extends Comparable<T>> T max(List<T> values) {
    if (values == null || values.isEmpty()) {
        throw new IllegalArgumentException("values must not be empty");
    }

    T result = values.get(0);
    for (T value : values) {
        if (value.compareTo(result) > 0) {
            result = value;
        }
    }
    return result;
}
```

`T extends Comparable<T>` nghĩa là T phải có khả năng compare với chính nó.

## 4. Wildcard

Wildcard `?` dùng khi bạn không cần hoặc không thể biết type cụ thể.

```java
public static void printAll(List<?> values) {
    for (Object value : values) {
        System.out.println(value);
    }
}
```

`List<?>` đọc được item như `Object`, nhưng không add item cụ thể được, trừ `null`.

## 5. PECS

PECS: Producer Extends, Consumer Super.

Nếu collection là nguồn đọc ra T, dùng `? extends T`.

```java
public static double total(List<? extends Number> numbers) {
    double sum = 0;
    for (Number number : numbers) {
        sum += number.doubleValue();
    }
    return sum;
}
```

Nếu collection là nơi ghi T vào, dùng `? super T`.

```java
public static void addDefaults(List<? super Integer> values) {
    values.add(1);
    values.add(2);
    values.add(3);
}
```

## 6. Type Erasure

Java generics chủ yếu tồn tại ở compile time. Khi runtime, nhiều thông tin generic bị xóa.

Không thể làm:

```java
// if (value instanceof List<String>) { } // không hợp lệ
// T item = new T();                      // không hợp lệ
```

Hệ quả:

- Generic giúp compiler bắt lỗi sớm.
- Runtime không luôn biết `List<String>` hay `List<Integer>`.
- Khi cần type runtime, thường truyền thêm `Class<T>` hoặc type token.


## 2. Enum Có Field Và Behavior

Enum trong Java có thể có field, constructor và method.

```java
public enum Plan {
    FREE(0),
    PRO(20),
    ENTERPRISE(100);

    private final int monthlyPrice;

    Plan(int monthlyPrice) {
        this.monthlyPrice = monthlyPrice;
    }

    public int monthlyPrice() {
        return monthlyPrice;
    }

    public boolean isPaid() {
        return monthlyPrice > 0;
    }
}
```

## 3. Enum Cho State Transition

```java
public enum TaskStatus {
    TODO,
    IN_PROGRESS,
    DONE;

    public boolean canMoveTo(TaskStatus next) {
        return switch (this) {
            case TODO -> next == IN_PROGRESS;
            case IN_PROGRESS -> next == DONE || next == TODO;
            case DONE -> false;
        };
    }
}
```

## 4. Static Nested Class

Static nested class không giữ reference tới instance bên ngoài.

```java
public class ApiResponse<T> {
    private final T data;
    private final Error error;

    public ApiResponse(T data, Error error) {
        this.data = data;
        this.error = error;
    }

    public static class Error {
        private final String code;
        private final String message;

        public Error(String code, String message) {
            this.code = code;
            this.message = message;
        }
    }
}
```

Dùng khi class phụ chỉ có nghĩa trong context class ngoài.

## 5. Inner Class

Inner class không static và giữ reference tới instance bên ngoài. Dùng ít hơn static nested class.

```java
public class Counter {
    private int value;

    public class View {
        public int value() {
            return value;
        }
    }
}
```

Cẩn thận: vì inner class giữ reference tới outer object, dùng sai có thể làm object sống lâu hơn cần thiết.

## 6. Anonymous Class

Anonymous class tạo implementation inline.

```java
Runnable task = new Runnable() {
    @Override
    public void run() {
        System.out.println("running");
    }
};
```

Với functional interface, lambda thường gọn hơn:

```java
Runnable task = () -> System.out.println("running");
```

Anonymous class vẫn hữu ích khi cần override nhiều method hoặc cần class nhỏ dùng một lần.


## Lưu ý khi đọc

- Quy tắc state transition ở đây là bài mở rộng, không thay đổi rule cho phép chuyển qua lại giữa ba status của Task Manager cơ bản.
- Inner class `View` đọc state hiện tại của Counter; nó không chụp lại một snapshot độc lập.
- Với `List<?>`, compiler chỉ cho thêm null, nhưng thao tác đó vẫn có thể bị list cụ thể từ chối ở runtime.

Tiếp theo: chọn đúng một khái niệm và tự viết caller trước khi đọc phần tiếp theo.
