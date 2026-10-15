# Record Và Switch: Viết Gọn Điều Bạn Đã Hiểu

> **Làm rõ trước khi học:** Record chỉ giữ cố định các field; không bảo đảm dữ liệu lồng bên trong immutable và không tự validate.

**Cách chạy ví dụ:** dùng JDK 21 (bộ công cụ Java). Mở terminal, gõ `jshell`; gõ từng khối Java độc lập, dùng `/reset` trước khối tiếp theo và `/exit` để thoát. JShell chạy được khai báo class, method (hàm của Java) và lệnh trực tiếp, không cần khung main. Các comment `//` giải thích dòng code và kết quả cần quan sát.

## A. Record Để Mang Dữ Liệu

Record là cách khai báo ngắn một class chủ yếu mang dữ liệu, có sẵn constructor, method đọc và so sánh giá trị.

```java
record TaskSummary(long id, String title) {} // Khai báo dữ liệu
TaskSummary task = new TaskSummary(1L, "Java"); // 1L là số long
System.out.println(task.title()); // Java: accessor (method đọc)
System.out.println(task.equals(new TaskSummary(1L, "Java"))); // true
```

**Liên hệ FE:** Giống object có shape rõ trong TS, nhưng record là class thực sự khi Java chạy.

**Khi nào dùng**

- Mang kết quả từ một method.
- Tạo DTO (object chỉ mang dữ liệu giữa các phần chương trình).

**Lỗi hay gặp**

- Nghĩ record tự kiểm tra title null.
- Nghĩ object chứa bên trong record không thể sửa.

**Quy tắc ngón tay cái:** Record bớt code khai báo; quy tắc dữ liệu vẫn do bạn viết.

**Tự gõ, không dùng AI sinh code:** Tạo record TaskCount chứa status kiểu String và count kiểu int. **Gợi ý:** Tạo hai giá trị cùng dữ liệu rồi một giá trị khác count; so equals.

## B. Switch Expression Để Chọn Một Kết Quả

Switch expression chọn và trả một kết quả theo từng trường hợp, thay chuỗi if/else đơn giản.

```java
enum Status { TODO, DONE }
Status status = Status.TODO;
String label = switch (status) { // Mỗi nhánh trả một String
    case TODO -> "Cần làm";
    case DONE -> "Xong";
};
System.out.println(label); // Cần làm
```

**Khi nào dùng**

- Đổi enum thành nhãn.
- Chọn một giá trị từ tập trường hợp rõ.

**Lỗi hay gặp**

- Bỏ sót trường hợp cần trả kết quả.
- Truyền null khi chưa có cách xử lý.

**Quy tắc ngón tay cái:** Cần chọn một kết quả: switch expression; luồng phức tạp có thể giữ if/else.

**Tự gõ, không dùng AI sinh code:** Thêm trạng thái IN_PROGRESS và nhãn tương ứng. **Gợi ý:** Đọc lỗi compiler trước khi bổ sung nhánh còn thiếu.

## Đọc thêm sau

[Sealed class, pattern matching, text block](06-advanced-reference.md). Học khi gặp code thật cần đọc; không phải điều kiện để tạo Task.
