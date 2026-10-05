# Java Hiện Đại: 17/21

## 1. Record

Record phù hợp cho data carrier immutable.

```java
public record UserDto(String id, String email) {
    public UserDto {
        if (id == null || id.isBlank()) {
            throw new IllegalArgumentException("id is required");
        }
        if (email == null || email.isBlank()) {
            throw new IllegalArgumentException("email is required");
        }
    }
}
```

Record tự tạo:

- constructor chính.
- accessor theo tên field: `id()`, `email()`.
- `equals`.
- `hashCode`.
- `toString`.

Dùng record khi:

- Object chủ yếu để mang dữ liệu.
- State immutable.
- Không cần inheritance.

Không dùng record khi:

- Object có identity phức tạp.
- Cần lifecycle mutable.
- Cần che giấu representation theo cách record không hợp.

## 2. Sealed Class

Sealed class giới hạn class nào được extend/implement.

```java
public sealed interface PaymentResult
        permits PaymentResult.Success, PaymentResult.Failed {

    record Success(String transactionId) implements PaymentResult {
    }

    record Failed(String reason) implements PaymentResult {
    }
}
```

Lợi ích:

- Mô hình hóa tập biến thể hữu hạn.
- Compiler hiểu được hierarchy.
- Kết hợp tốt với switch expression và pattern matching.

## 3. Pattern Matching Cho `instanceof`

Cũ:

```java
if (value instanceof String) {
    String text = (String) value;
    System.out.println(text.length());
}
```

Mới:

```java
if (value instanceof String text) {
    System.out.println(text.length());
}
```

## 4. Switch Expression

```java
public int priority(TaskStatus status) {
    return switch (status) {
        case TODO -> 1;
        case IN_PROGRESS -> 2;
        case BLOCKED -> 3;
        case DONE -> 0;
    };
}
```

So với switch statement cũ:

- Trả về value.
- Ít lỗi quên `break`.
- Dễ đọc khi mapping enum sang value.

## 5. Text Block

Text block dùng cho string nhiều dòng.

```java
String json = """
    {
      "name": "Java",
      "version": 21
    }
    """;
```

Dùng tốt cho:

- JSON sample trong test.
- SQL query nhỏ.
- Template text.

## 6. Bài Tập Nhanh

Thiết kế result type cho parser:

- `ParseResult.Success<T>`.
- `ParseResult.Failure`.
- Dùng sealed interface + record.

Sau đó viết method:

```java
String render(ParseResult<?> result)
```

Yêu cầu:

- Dùng pattern matching nếu Java version hỗ trợ.
- Dùng switch expression nếu phù hợp.
- Có test cho success/failure.
