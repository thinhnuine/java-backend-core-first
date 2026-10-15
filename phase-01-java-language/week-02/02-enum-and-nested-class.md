# Enum: Chọn Trạng Thái Từ Một Tập Hợp Cố Định

> **Làm rõ trước khi học:** Enum chỉ giới hạn lựa chọn, không tự định nghĩa “DONE không được đổi nữa”. Quy tắc chuyển trạng thái phải được viết riêng.

Học một ví dụ rồi dừng để tự sửa đầu vào. Các heading giữ nguyên để đối chiếu bản trước.

**Cách chạy ví dụ:** dùng JDK 21 (bộ công cụ Java). Mở terminal, gõ `jshell`; gõ từng khối Java độc lập, dùng `/reset` trước khối tiếp theo và `/exit` để thoát. JShell chạy được khai báo class, method (hàm của Java) và lệnh trực tiếp, không cần khung main. Các comment `//` giải thích dòng code và kết quả cần quan sát.

## 1. Ý chính

Enum là kiểu có một tập giá trị đặt tên sẵn, giúp trạng thái không bị sai do gõ nhầm chuỗi.

## 2. Giải thích code

```java
enum TaskStatus { TODO, IN_PROGRESS, DONE } // Ba lựa chọn hợp lệ
TaskStatus status = TaskStatus.TODO;
status = TaskStatus.DONE; // Đổi lựa chọn của biến
System.out.println(status == TaskStatus.DONE); // true: enum so bằng ==
```

## 3. Vì sao thiết kế như vậy

String "DONNE" vẫn là chuỗi hợp lệ; TaskStatus.DONNE không tồn tại nên compiler báo lỗi. **Quy tắc ngón tay cái:** lựa chọn cố định do code quyết định thì cân nhắc enum.

## 4. Liên hệ với frontend

Gần với TS union `"TODO" | "DONE"`. Enum Java tồn tại khi chạy; TS union không tự kiểm tra dữ liệu nhận từ mạng.

## 5. Khi nào dùng và không dùng

**Khi nào dùng**

- Trạng thái task có số lựa chọn rõ.
- Loại thao tác cố định trong chương trình.

## 6. Bẫy hay gặp

**Lỗi hay gặp**

- Gán chuỗi "DONE" vào biến TaskStatus.
- Nghĩ enum không thể null.
- Lưu ordinal (vị trí hằng) làm mã bền vững.

## 7. Thuật ngữ mới

**Enum constant** là một giá trị có tên như DONE. **Runtime** là lúc chương trình chạy. **Nested class** là class khai báo bên trong class khác; để sau.

## 8. Bài tập nhỏ để tự gõ lại

**Tự gõ, không dùng AI sinh code:** Viết method label nhận TaskStatus và trả nhãn tiếng Việt.

**Gợi ý:** Dùng if/else; tự quy định cách xử lý null. Task Manager cơ bản cho đổi qua lại ba trạng thái.

## Đọc thêm sau

[Nested class và enum có hành vi](06-advanced-reference.md). valueOf yêu cầu tên khớp chính xác; chỉ học chuyển từ chuỗi khi cần nhận dữ liệu ngoài.
