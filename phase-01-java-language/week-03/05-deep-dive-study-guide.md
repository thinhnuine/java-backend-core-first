# Mentor Guide Tuần 3: Modern Java, Exception Và Capstone

> **Đọc sau:** đây là mentor guide mở rộng của giáo trình cũ, không phải điểm bắt đầu hay checklist bắt buộc. Học theo [README mới](README.md) và hoàn thành bài cơ bản trước.

## 1. Ý chính

Tuần này học các feature Java hiện đại như `record`, `sealed class`, `pattern matching`, `switch expression` và cách xử lý exception. Mục tiêu không phải dùng feature mới cho "ngầu", mà là biết khi nào chúng làm domain model rõ hơn. Cuối tuần bạn chọn một capstone nhỏ để gom lại kiến thức tuần 1-3.

## 2. Giải thích code

Ví dụ `record`:

```java
package dev.thinh.javacore;

public record EmailAddress(String value) {
    public EmailAddress {
        if (value == null || value.isBlank()) {
            throw new IllegalArgumentException("email is required");
        }
    }
}
```

Giải thích:

- `record`: data carrier có component field final; bất biến chỉ ở mức nông nếu component tham chiếu object mutable.
- `EmailAddress(String value)`: component của record. Java tự tạo field private final, constructor, accessor `value()`, `equals`, `hashCode`, `toString`.
- `public EmailAddress { ... }`: compact constructor, dùng để validate.
- Không cần viết `this.value = value`; record tự gán sau block validation.

Ví dụ sealed result:

```java
public sealed interface ParseResult<T>
        permits ParseResult.Success, ParseResult.Failure {

    record Success<T>(T value) implements ParseResult<T> {
    }

    record Failure<T>(String message) implements ParseResult<T> {
    }
}
```

- `sealed interface`: chỉ các class/record trong `permits` được implement.
- `permits`: liệt kê biến thể hợp lệ.
- `record Success<T>` và `record Failure<T>` là hai kết quả có thể xảy ra.

Ví dụ switch expression:

```java
String label = switch (status) {
    case TODO -> "Todo";
    case IN_PROGRESS -> "In progress";
    case DONE -> "Done";
};
```

`switch` này trả về value, giảm lỗi quên `break`.

## 3. Vì sao thiết kế như vậy

Không dùng `record`, bạn phải viết nhiều boilerplate cho DTO/value object:

```java
public final class UserDto {
    private final String id;
    private final String email;

    public UserDto(String id, String email) {
        this.id = id;
        this.email = email;
    }

    public String id() {
        return id;
    }

    public String email() {
        return email;
    }
}
```

`record` giúp code ngắn nhưng vẫn rõ.

Không dùng sealed type, bạn có thể model parse result bằng `null`:

```java
JsonValue value = parser.parse(input);
if (value == null) {
    // parse failed, but why?
}
```

Thiết kế này mơ hồ. `ParseResult.Success`/`Failure` nói rõ success hay failure và mang data phù hợp.

## 4. Liên hệ với Frontend

`record` hơi giống TypeScript object type ở mặt data shape:

```ts
type UserDto = {
  id: string
  email: string
}
```

Nhưng Java record là class thật, có constructor validation và method runtime.

`sealed interface` gần với discriminated union trong TypeScript:

```ts
type ParseResult<T> =
  | { kind: "success"; value: T }
  | { kind: "failure"; message: string }
```

Điểm khác: Java dùng type hierarchy thay vì field `kind`.

## 5. Khi nào dùng và không dùng

| Feature | Khi dùng | Khi tránh |
| --- | --- | --- |
| `record` | DTO, value object đơn giản, data carrier | Object cần mutable lifecycle |
| `sealed` | Tập biến thể hữu hạn như result/state | Khi extension bên ngoài là yêu cầu |
| pattern matching | Check type rồi dùng luôn biến typed | Logic quá phức tạp nên refactor |
| switch expression | Mapping enum/sealed type sang value | Branch có side effect dài |
| checked exception | Caller có thể recover rõ ràng | Lỗi programming/invalid argument |
| unchecked exception | Invalid argument/state, domain error nhỏ | Lỗi I/O caller cần xử lý |

## 6. Bẫy hay gặp

- Dùng record nhưng component là mutable list rồi tưởng object immutable tuyệt đối.
- Dùng sealed type cho case enum là đủ.
- Catch exception rồi không log/không rethrow.
- Throw `Exception` chung chung.
- Custom exception message quá mơ hồ.
- Capstone scope quá lớn, chưa xong được.

## 7. Thuật ngữ mới

- `record`: data carrier với component field final; Java sinh constructor/accessor/equality/toString, không tự validate hoặc deep-freeze.
- `compact constructor`: constructor ngắn của record để validate.
- `sealed`: giới hạn class nào được extend/implement.
- `pattern matching`: check type và bind biến trong một bước.
- `switch expression`: switch trả value.
- `checked exception`: exception bắt buộc catch hoặc declare.
- `unchecked exception`: exception runtime không bắt buộc catch.

## 8. Bài tập nhỏ để tự gõ lại

Chọn một capstone:

1. JSON parser đơn giản: hợp học `record` + `sealed`.
2. LRU cache: hợp học generics + collections.
3. Event emitter: hợp học interface + functional style.

Trước khi code, viết README ngắn:

- API public dự kiến.
- Scope làm và không làm.
- Exception nào sẽ throw.
- Test case đầu tiên.

Học tiếp theo: nếu chọn JSON parser, hãy viết model `JsonValue` bằng sealed interface trước. Nếu chọn LRU, bắt đầu bằng `LinkedHashMap` rồi mới tự viết linked list sau.

Phân loại checked/unchecked dựa trên hierarchy exception, không chỉ dựa vào khả năng recover: RuntimeException/Error và subclass là unchecked. Dòng hướng dẫn “có thể recover” là gợi ý thiết kế, không phải quy tắc compiler.
