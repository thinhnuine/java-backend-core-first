# `java.time` Và NIO.2

**Cách chạy ví dụ:** dùng JDK 21 (bộ công cụ Java). Mở terminal, gõ `jshell`; gõ từng khối Java độc lập, dùng `/reset` trước khối tiếp theo và `/exit` để thoát. JShell chạy được khai báo class, method (hàm của Java) và lệnh trực tiếp, không cần khung main. Các comment `//` giải thích dòng code và kết quả cần quan sát.

## `java.time`

java.time là nhóm kiểu dữ liệu biểu diễn ngày, giờ và khoảng thời gian.

```java
var dueDate = java.time.LocalDate.of(2026, 10, 20); // Ngày không kèm giờ
System.out.println(dueDate.plusDays(1)); // 2026-10-21
System.out.println(dueDate); // 2026-10-20: giá trị cũ không đổi
```

**Khi nào dùng**

- Ngày hết hạn task.
- Thời điểm tạo bản ghi.

**Lỗi hay gặp**

- Nghĩ plusDays sửa object cũ.

**Quy tắc ngón tay cái:** Ngày không giờ: LocalDate; một thời điểm: Instant.

**Tự gõ, không dùng AI sinh code:** Tạo ngày hết hạn rồi cộng 7 ngày. **Gợi ý:** Giữ kết quả plusDays vào biến mới.

## Chọn Type

Chọn type (kiểu dữ liệu) theo ý nghĩa thời gian, để tránh tự thêm múi giờ vào một ngày thuần túy.

```java
var createdAt = java.time.Instant.parse("2026-10-15T00:00:00Z"); // Một thời điểm
var birthday = java.time.LocalDate.parse("2000-01-01"); // Một ngày
System.out.println(createdAt);
System.out.println(birthday);
```

**Khi nào dùng**

- Thiết kế field ngày giờ.
- Đổi dữ liệu từ API sang Java.

**Lỗi hay gặp**

- Dùng LocalDateTime và tưởng đã có múi giờ.

**Tự gõ, không dùng AI sinh code:** Chọn kiểu cho createdAt và dueDate của Task. **Gợi ý:** Hỏi dữ liệu cần giờ và một thời điểm tuyệt đối hay chỉ cần ngày.

## Parse Và Format

Parse là đọc chuỗi thành ngày; format là viết ngày thành chuỗi theo mẫu.

```java
var format = java.time.format.DateTimeFormatter.ofPattern("dd/MM/uuuu");
var date = java.time.LocalDate.parse("15/10/2026", format); // Đọc theo mẫu
System.out.println(date); // 2026-10-15
System.out.println(date.format(format)); // 15/10/2026
```

**Khi nào dùng**

- Đọc ngày từ chuỗi input.
- Hiển thị ngày theo mẫu chọn trước.

**Lỗi hay gặp**

- Mẫu không khớp chuỗi.
- Nghĩ format thay đổi dữ liệu ngày bên trong.

**Tự gõ, không dùng AI sinh code:** Dùng một mẫu năm-tháng-ngày khác. **Gợi ý:** Đổi cả chuỗi đầu vào và mẫu cho khớp.

## NIO.2

NIO.2 là nhóm công cụ làm việc với đường dẫn và file; Path biểu diễn đường dẫn, Files thực hiện thao tác.

```java
var path = java.nio.file.Files.createTempFile("java-lesson-", ".txt");
java.nio.file.Files.writeString(path, "Hello"); // Ghi file tạm riêng
System.out.println(java.nio.file.Files.readString(path)); // Hello
java.nio.file.Files.delete(path); // Dọn file mẫu vừa tạo
```

**Khi nào dùng**

- Đọc dữ liệu file nhỏ.
- Ghi báo cáo bài tập.

**Lỗi hay gặp**

- Ghi đè đường dẫn chứa dữ liệu thật.
- Quên thao tác file có thể ném lỗi IOException.

**Tự gõ, không dùng AI sinh code:** Ghi hai dòng vào file tạm rồi đọc lại. **Gợi ý:** Dùng ký tự xuống dòng \n trong chuỗi.

## File Lớn

Với file lớn, đọc từng dòng giúp tránh giữ toàn bộ nội dung trong bộ nhớ.

```java
var path = java.nio.file.Files.createTempFile("java-lines-", ".txt");
java.nio.file.Files.writeString(path, "INFO\nERROR\n");
try (var lines = java.nio.file.Files.lines(path)) { // Tự đóng file khi xong
    System.out.println(lines.filter(line -> line.contains("ERROR")).count()); // 1
}
java.nio.file.Files.delete(path);
```

**Khi nào dùng**

- Đếm dòng trong file log (file ghi hoạt động).
- Xử lý file không cần giữ hết nội dung.

**Lỗi hay gặp**

- Gọi toList rồi nghĩ không còn tải toàn bộ.
- Không đóng stream từ Files.lines.

**Quy tắc ngón tay cái:** Đọc từng dòng chỉ tiết kiệm nếu không gom tất cả trở lại bộ nhớ.

**Tự gõ, không dùng AI sinh code:** Đếm dòng INFO thay vì ERROR. **Gợi ý:** Đổi điều kiện filter; giữ khối try để đóng tài nguyên.

## Bài Tập Nhanh

Tập đọc một dòng log trước khi làm cả file.

- Tách timestamp (chuỗi thời điểm) khỏi phần nội dung.
- Parse timestamp bằng Instant.parse.
- Báo rõ khi dòng không hợp lệ.

**Gợi ý:** Chọn một định dạng đơn giản, viết ví dụ hợp lệ và không hợp lệ trước.

**Đạt khi:** Một dòng đúng đọc được; một dòng sai cho lỗi bạn giải thích được. Tự gõ phần triển khai, không dùng AI sinh lời giải.

## Đọc thêm sau

ZonedDateTime giữ ngày giờ cùng múi giờ; Duration đo khoảng thời gian theo giây, Period theo ngày/tháng/năm. DST (đổi giờ mùa) và định dạng nghiêm ngặt học khi cần lịch theo vùng.
