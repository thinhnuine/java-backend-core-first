# `java.time` Và NIO.2

## `java.time`

Ưu tiên API hiện đại thay vì `Date`/`Calendar`.

Các type chính:

- `Instant`: thời điểm tuyệt đối.
- `LocalDate`: ngày không timezone.
- `LocalDateTime`: ngày giờ không timezone.
- `ZonedDateTime`: ngày giờ có timezone.
- `Duration`: khoảng thời gian theo giây/nano.
- `Period`: khoảng thời gian theo ngày/tháng/năm.

## Chọn Type

Dùng `Instant` cho:

- createdAt/updatedAt trong backend.
- event timestamp.
- audit log.

Dùng `LocalDate` cho:

- ngày sinh.
- due date không cần giờ.
- ngày nghỉ.

Dùng `ZonedDateTime` khi:

- hiển thị hoặc xử lý theo timezone cụ thể.
- lịch họp, lịch đặt chỗ.

## Parse Và Format

```java
Instant timestamp = Instant.parse("2026-10-05T10:00:01Z");
LocalDate date = LocalDate.parse("2026-10-05");
```

Formatter:

```java
DateTimeFormatter formatter = DateTimeFormatter.ofPattern("yyyy/MM/dd");
LocalDate date = LocalDate.parse("2026/10/05", formatter);
```

## NIO.2

API chính:

- `Path`
- `Files.readString`
- `Files.lines`
- `Files.newBufferedReader`
- `Files.walk`
- `Files.writeString`

Ví dụ:

```java
Path path = Path.of("app.log");
try (Stream<String> lines = Files.lines(path)) {
    long errors = lines.filter(line -> line.contains("ERROR")).count();
}
```

`Files.lines` cần try-with-resources vì stream giữ resource file.

## File Lớn

Tránh:

```java
List<String> lines = Files.readAllLines(path);
```

Khi file lớn, dùng:

- `Files.lines`.
- `Files.newBufferedReader`.
- xử lý từng dòng.

## Bài Tập Nhanh

Viết `LogLineParser`:

Input:

```text
2026-10-05T10:00:01Z INFO user=1 action=login latencyMs=32
```

Output record:

```java
public record LogLine(
    Instant timestamp,
    String level,
    String user,
    String action,
    long latencyMs
) {
}
```
