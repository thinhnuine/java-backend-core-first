# Tra Cứu Sau: Sealed Type Và Exception Mở Rộng

Đọc sau checklist cơ bản. Đây là snippet cần class/import/caller tương ứng. Không bắt buộc làm parser để bắt đầu Spring.

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


## 4. Try-With-Resources

Dùng cho resource cần close như file stream, socket, database connection.

```java
try (BufferedReader reader = Files.newBufferedReader(Path.of("app.log"))) {
    String line;
    while ((line = reader.readLine()) != null) {
        System.out.println(line);
    }
}
```

Resource phải implement `AutoCloseable`.

## 5. Custom Exception

```java
public class InvalidJsonException extends RuntimeException {
    public InvalidJsonException(String message) {
        super(message);
    }

    public InvalidJsonException(String message, Throwable cause) {
        super(message, cause);
    }
}
```

Quy tắc:

- Exception message phải đủ thông tin debug.
- Đừng nuốt exception.
- Khi wrap exception, giữ `cause`.
- Đừng dùng exception cho control flow bình thường.


Tiếp theo: thử một text block trong test, hoặc đọc try-with-resources khi bắt đầu làm file/JDBC.
