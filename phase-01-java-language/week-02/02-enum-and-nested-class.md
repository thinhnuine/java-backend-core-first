# Enum Và Nested Class

## 1. Enum Cơ Bản

Enum dùng cho tập giá trị hữu hạn.

```java
public enum OrderStatus {
    NEW,
    PAID,
    SHIPPED,
    CANCELLED
}
```

Không nên dùng string tự do cho trạng thái quan trọng vì dễ typo và khó refactor.

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

    public class Snapshot {
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

## 7. Bài Tập Nhanh

Thiết kế `WorkflowStatus` cho task:

- `TODO`
- `IN_PROGRESS`
- `BLOCKED`
- `DONE`
- `CANCELLED`

Yêu cầu:

- Có method `canMoveTo`.
- Có method `isTerminal`.
- Có test cho transition hợp lệ và không hợp lệ.
