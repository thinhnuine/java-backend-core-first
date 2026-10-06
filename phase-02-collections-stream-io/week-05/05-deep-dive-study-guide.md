# Mentor Guide Tuần 5: Stream, Optional, Date/Time Và I/O

## 1. Ý chính

`Stream` giúp bạn xử lý collection như một pipeline dữ liệu: lọc, biến đổi, gom kết quả. `Optional` giúp biểu diễn kết quả có thể vắng mặt mà không phải trả `null`. `java.time` và NIO.2 giúp bạn xử lý thời gian và file theo cách hiện đại hơn.

## 2. Giải thích code

Ví dụ Stream:

```java
List<String> activeEmails = users.stream()
    .filter(User::active)
    .map(User::email)
    .sorted()
    .toList();
```

Giải thích:

- `users.stream()`: tạo stream từ list.
- `filter(User::active)`: giữ user active.
- `map(User::email)`: đổi `User` thành email.
- `sorted()`: sort email.
- `toList()`: terminal operation, gom kết quả thành list.

Ví dụ Optional:

```java
User user = repository.findById("u1")
    .orElseThrow(() -> new IllegalArgumentException("user not found"));
```

- `findById` trả `Optional<User>`.
- `orElseThrow`: nếu có user thì lấy ra, nếu không có thì throw exception.

Ví dụ đọc file:

```java
try (Stream<String> lines = Files.lines(Path.of("app.log"))) {
    long errors = lines.filter(line -> line.contains("ERROR")).count();
    System.out.println(errors);
}
```

- `Files.lines`: đọc file dạng stream từng dòng.
- `try-with-resources`: đảm bảo đóng file.
- `count()`: terminal operation.

## 3. Vì sao thiết kế như vậy

Stream làm code rõ khi logic là pipeline. Nhưng nếu bạn nhồi side effect phức tạp vào stream, code sẽ khó debug.

Ví dụ stream xấu:

```java
users.stream()
    .map(user -> {
        audit(user);
        sendEmail(user);
        return user.email();
    })
    .toList();
```

Đoạn này vừa transform vừa gây side effect. Loop thường rõ hơn.

Optional giúp tránh null mơ hồ. Code xấu:

```java
User user = repository.findByIdOrNull("u1");
System.out.println(user.email()); // NullPointerException nếu không có user
```

Code rõ hơn:

```java
User user = repository.findById("u1")
    .orElseThrow(() -> new IllegalArgumentException("user not found"));
```

## 4. Liên hệ với Frontend

Stream giống chain array method trong JS:

```javascript
users
  .filter(user => user.active)
  .map(user => user.email)
  .sort()
```

Optional hơi giống kiểu `User | undefined` trong TypeScript, nhưng Java buộc bạn xử lý qua API như `map`, `orElse`, `orElseThrow`.

`Instant` giống timestamp chuẩn để backend lưu event time. Khi hiển thị cho user, frontend mới format theo locale/timezone.

## 5. Khi nào dùng và không dùng

| Công cụ | Khi dùng | Khi tránh |
| --- | --- | --- |
| Stream | Pipeline filter/map/group rõ ràng | Logic nhiều side effect |
| Loop | Cần debug từng bước, logic phức tạp | Pipeline đơn giản có thể viết gọn |
| Optional return | Kết quả có thể không tồn tại | Field/parameter trong đa số case |
| `Instant` | Timestamp backend | Ngày sinh/due date không giờ |
| `LocalDate` | Ngày không timezone | Event timestamp |
| `Files.lines` | File lớn, đọc từng dòng | Quên close stream |

## 6. Bẫy hay gặp

- Gọi `Optional.get()` bừa bãi.
- Stream quá dài, khó đọc.
- Dùng `forEach` để mutate state ngoài.
- Dùng `readAllLines` với file rất lớn.
- Dùng sai type thời gian, ví dụ due date lại dùng `Instant`.
- Quên `try-with-resources` khi dùng `Files.lines`.

## 7. Thuật ngữ mới

- `stream`: dòng xử lý dữ liệu.
- `intermediate operation`: operation trả stream mới như `filter`, `map`.
- `terminal operation`: operation kết thúc stream như `toList`, `count`.
- `Optional`: container biểu diễn có/không có value.
- `Instant`: thời điểm tuyệt đối.
- `NIO.2`: API file/path hiện đại của Java.

## 8. Bài tập nhỏ để tự gõ lại

Viết `LogLineParser` và `LogProcessor`:

1. Parse dòng log thành `LogLine`.
2. Đếm số dòng `ERROR`.
3. Group count theo level bằng Stream.
4. Viết lại bằng loop.
5. Ghi chú bản nào dễ đọc hơn.

Học tiếp theo: khi code Stream, nếu bạn không giải thích pipeline thành một câu tiếng Việt được, hãy thử viết loop trước.
