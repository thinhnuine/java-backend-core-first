# Exception: Khi Method Không Thể Hoàn Thành Công Việc

**Cách chạy ví dụ:** dùng JDK 21 (bộ công cụ Java). Mở terminal, gõ `jshell`; gõ từng khối Java độc lập, dùng `/reset` trước khối tiếp theo và `/exit` để thoát. JShell chạy được khai báo class, method (hàm của Java) và lệnh trực tiếp, không cần khung main. Các comment `//` giải thích dòng code và kết quả cần quan sát.

## A. Hiểu Luồng Chạy Trước Khi Phân Loại Exception

Exception là cách báo một thao tác không hoàn thành bình thường; throw phát sinh lỗi, catch nhận lỗi để xử lý.

```java
int positive(int value) {
    if (value < 0) throw new IllegalArgumentException("negative"); // Từ chối input
    return value;
}
try { // Chạy thao tác có thể thất bại
    System.out.println(positive(-1));
} catch (IllegalArgumentException error) {
    System.out.println(error.getMessage()); // negative
}
```

**Khi nào dùng**

- Từ chối tham số vi phạm quy tắc.
- Báo thao tác đọc file hoặc lưu dữ liệu thất bại.

**Lỗi hay gặp**

- Catch rồi bỏ qua mà không biết có nên tiếp tục.
- Dùng exception thay cho mọi if thông thường.

**Quy tắc ngón tay cái:** Chỉ catch ở nơi biết cách phản ứng với lỗi.

**Tự gõ, không dùng AI sinh code:** Viết method từ chối title null hoặc trắng; gọi cả đầu vào đúng và sai. **Gợi ý:** Kiểm tra null trước isBlank; đọc message của lỗi.

## B. Checked/Unchecked Và Sự Khác Nhau Giữa Throw/Throws

Checked exception buộc bạn xử lý hoặc khai báo bằng throws; unchecked exception không có yêu cầu đó khi biên dịch.

```java
void readDemo() throws java.io.IOException { // Khai báo lỗi có thể truyền ra
    throw new java.io.IOException("demo failure"); // Phát sinh lỗi thật
}
try { readDemo(); }
catch (java.io.IOException error) {
    System.out.println(error.getMessage()); // demo failure
}
```

**Khi nào dùng**

- Dùng throws để bên gọi quyết định cách xử lý checked exception.
- Dùng IllegalArgumentException cho tham số không hợp lệ.

**Lỗi hay gặp**

- Nhầm throws là lệnh phát sinh lỗi.
- Nghĩ unchecked nghĩa là không cần quan tâm.

**Quy tắc ngón tay cái:** throw là hành động; throws là lời báo trước.

**Tự gõ, không dùng AI sinh code:** Bỏ throws khỏi readDemo để xem compiler yêu cầu gì; sau đó khôi phục. **Gợi ý:** So vị trí throw trong thân method và throws ở phần khai báo.

## Đọc thêm sau

Try-with-resources (tự đóng tài nguyên), custom exception (loại lỗi tự đặt) ở [tra cứu sau](06-advanced-reference.md). Không bắt Error để tiếp tục như thể ứng dụng còn hoạt động bình thường.
