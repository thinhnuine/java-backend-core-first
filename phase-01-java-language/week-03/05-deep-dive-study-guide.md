# Deep Dive Tuần 3: Modern Java, Exception Và Capstone

## Cách Học Tuần Này

Tuần này nối hai thứ: cú pháp Java hiện đại và khả năng thiết kế một thư viện nhỏ. Bạn không cần dùng mọi feature mới ở mọi nơi. Mục tiêu là biết feature nào làm model rõ hơn.

Tư duy chính:

- Record dùng cho data carrier immutable.
- Sealed type dùng cho tập biến thể hữu hạn.
- Switch expression dùng cho mapping rõ ràng.
- Exception là một phần của contract.
- Capstone là bài tổng hợp, không phải bài khoe nhiều feature.

## Record

Record giúp viết value/data object ngắn hơn.

Class thường:

```java
public final class UserDto {
    private final String id;
    private final String email;
}
```

Record:

```java
public record UserDto(String id, String email) {
}
```

Record tự sinh constructor, accessor, `equals`, `hashCode`, `toString`.

Dùng record khi object chủ yếu để mang dữ liệu. Không dùng record nếu object có lifecycle mutable phức tạp.

## Compact Constructor

Record vẫn validate được:

```java
public record EmailAddress(String value) {
    public EmailAddress {
        if (value == null || value.isBlank()) {
            throw new IllegalArgumentException("email is required");
        }
    }
}
```

Bạn không cần viết `this.value = value`; record tự làm sau block validation.

## Sealed Type

Sealed type hợp khi bạn biết toàn bộ biến thể hợp lệ.

Ví dụ parser:

```java
public sealed interface ParseResult<T>
        permits ParseResult.Success, ParseResult.Failure {

    record Success<T>(T value) implements ParseResult<T> {
    }

    record Failure<T>(String message) implements ParseResult<T> {
    }
}
```

Lợi ích: code đọc thấy rõ result chỉ có success hoặc failure.

## Switch Expression

Switch expression trả value:

```java
int priority = switch (status) {
    case TODO -> 1;
    case IN_PROGRESS -> 2;
    case DONE -> 0;
};
```

Nó giảm lỗi quên `break` so với switch statement cũ.

## Exception Là Contract

Exception không chỉ là "báo lỗi". Nó nói cho caller biết method có thể fail như thế nào.

Unchecked exception hợp với lỗi do caller dùng sai API:

```java
throw new IllegalArgumentException("currency must match");
```

Checked exception hợp khi caller có khả năng recover rõ ràng, ví dụ file user chọn không tồn tại.

Quy tắc thực dụng giai đoạn này:

- Validate argument sai: `IllegalArgumentException`.
- State object sai: `IllegalStateException`.
- Parse input sai: custom unchecked exception cũng ổn cho bài nhỏ.
- I/O thật: dùng hoặc wrap `IOException` có chủ đích.

## Try-With-Resources

Resource cần close nên dùng try-with-resources:

```java
try (BufferedReader reader = Files.newBufferedReader(path)) {
    return reader.readLine();
}
```

Java sẽ close resource kể cả khi có exception.

## Capstone Nên Chọn Gì?

Nếu muốn học type system và generics: chọn LRU cache.

Nếu muốn học sealed/record/parser thinking: chọn JSON parser.

Nếu muốn gần frontend/event mindset: chọn Event Emitter.

Không chọn bài quá to. Một thư viện nhỏ, API rõ, test tốt đáng giá hơn một project ôm đồm.

## Bài Tập Theo Bước

1. Chọn capstone.
2. Viết README trước: thư viện làm gì, API dự kiến.
3. Viết public API tối thiểu.
4. Viết test happy path.
5. Implement phần nhỏ nhất.
6. Thêm edge case.
7. Viết note tradeoff.

## Lỗi Thường Gặp

- Dùng record cho object cần mutable lifecycle.
- Dùng sealed type chỉ vì mới, dù enum đủ.
- Catch exception rồi nuốt mất lỗi.
- Custom exception message quá chung chung.
- Capstone scope quá lớn.

## Câu Hỏi Tự Kiểm Tra

- Record tự sinh những method nào?
- Record có immutable tuyệt đối không nếu field là `List` mutable?
- Sealed type khác enum ở đâu?
- Exception nào là do caller sai input?
- Try-with-resources giải quyết vấn đề gì?
- Capstone của bạn có public API đủ nhỏ chưa?
