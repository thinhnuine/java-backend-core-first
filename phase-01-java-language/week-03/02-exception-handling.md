# Exception Handling

## 1. Checked Vs Unchecked Exception

Checked exception phải được catch hoặc declare bằng `throws`.

```java
public String readFile(String path) throws IOException {
    return Files.readString(Path.of(path));
}
```

Unchecked exception extends `RuntimeException`, không bắt buộc catch/declare.

```java
public Money(long amount, String currency) {
    if (amount < 0) {
        throw new IllegalArgumentException("amount must be non-negative");
    }
    this.amount = amount;
    this.currency = currency;
}
```

## 2. Khi Nào Dùng Checked

Dùng checked exception khi:

- Caller có khả năng recover hợp lý.
- Lỗi là một phần rõ ràng của contract.
- Bạn muốn ép caller xử lý.

Ví dụ:

- File không tồn tại khi import file do user chọn.
- Network call tới service ngoài thất bại và caller có fallback.

## 3. Khi Nào Dùng Unchecked

Dùng unchecked exception khi:

- Lỗi do bug hoặc invalid programming usage.
- Caller thường không recover ngay tại chỗ.
- Invariant bị vi phạm.

Ví dụ:

- Argument null không hợp lệ.
- State object không đúng.
- Không tìm thấy config bắt buộc khi app khởi động.

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

## 6. Bài Tập Nhanh

Viết `ConfigLoader`.

Yêu cầu:

- Nhận path tới file config text.
- Dùng `try-with-resources` hoặc `Files.readString`.
- Nếu file không tồn tại, trả lỗi rõ ràng.
- Nếu format sai, throw custom exception `InvalidConfigException`.
- Có test cho file hợp lệ, file thiếu, format sai.

Tự quyết:

- `InvalidConfigException` nên checked hay unchecked?
- Vì sao?
