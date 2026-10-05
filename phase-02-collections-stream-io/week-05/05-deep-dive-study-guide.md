# Deep Dive Tuần 5: Stream, Optional, Date/Time Và I/O

## Cách Học Tuần Này

Stream không phải để thay mọi vòng lặp. Nó là công cụ để diễn đạt pipeline xử lý dữ liệu. Nếu pipeline làm code rõ hơn, dùng. Nếu làm debug khó hơn, loop vẫn rất đáng kính.

## Stream Mental Model

Stream là dòng dữ liệu đi qua các bước:

```text
source -> filter -> map -> collect
```

Ví dụ:

```java
users.stream()
    .filter(User::active)
    .map(User::email)
    .toList();
```

Đọc thành câu: từ users, giữ user active, lấy email, gom thành list.

## Intermediate Vs Terminal Operation

Intermediate operation trả stream mới:

- `filter`
- `map`
- `flatMap`
- `sorted`
- `distinct`

Terminal operation kết thúc stream:

- `toList`
- `collect`
- `count`
- `findFirst`
- `forEach`

Stream lazy: intermediate operation chưa chạy cho tới khi có terminal operation.

## Optional

`Optional` biểu diễn kết quả có thể vắng mặt.

Tốt:

```java
repository.findById(id)
    .orElseThrow(() -> new UserNotFoundException(id));
```

Không tốt:

```java
repository.findById(id).get();
```

Nếu bạn gọi `.get()` không check, bạn gần như quay lại lỗi null nhưng bằng cú pháp khác.

## java.time

Đừng dùng `Date`/`Calendar` cho code mới.

Chọn nhanh:

- `Instant`: timestamp backend/audit/event.
- `LocalDate`: ngày không cần giờ/timezone.
- `LocalDateTime`: ngày giờ không timezone, dùng cẩn thận.
- `ZonedDateTime`: ngày giờ có timezone.
- `Duration`: khoảng thời gian kiểu 5 phút.
- `Period`: khoảng thời gian kiểu 2 tháng.

## NIO.2 Và File Lớn

Với file nhỏ:

```java
String content = Files.readString(path);
```

Với file lớn:

```java
try (Stream<String> lines = Files.lines(path)) {
    // process
}
```

Đừng `readAllLines` với file lớn nếu không cần load toàn bộ vào memory.

## Log Processor Nên Thiết Kế Ra Sao

Tách 3 phần:

- parser: string -> `LogLine`
- processor/aggregator: nhiều `LogLine` -> summary
- reporter: summary -> text/report

Đừng để một method vừa đọc file, parse, tính toán, format output dài 200 dòng.

## Bài Tập Theo Bước

1. Parse một dòng log thành `LogLine`.
2. Viết test cho parser.
3. Viết loop version xử lý list dòng nhỏ.
4. Viết stream version xử lý cùng input.
5. Đổi input sang file.
6. Viết report so sánh.

## Lỗi Thường Gặp

- Stream pipeline quá dài.
- Dùng `forEach` với side effect phức tạp.
- Quên close `Files.lines`.
- Dùng sai time type.
- Dùng Optional cho field/parameter quá mức.

## Câu Hỏi Tự Kiểm Tra

- Pipeline stream của bạn có đọc thành câu được không?
- Nếu debug từng dòng thì Stream hay loop dễ hơn?
- Optional đang giúp rõ nghĩa hay chỉ làm code dài hơn?
- File 5GB thì code hiện tại có sống được không?
- Timestamp trong log nên dùng type nào?
