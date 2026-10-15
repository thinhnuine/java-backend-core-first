# Tra Cứu Sau: Sealed Type Và Exception Mở Rộng

**Cách chạy ví dụ:** dùng JDK 21 (bộ công cụ Java). Mở terminal, gõ `jshell`; gõ từng khối Java độc lập, dùng `/reset` trước khối tiếp theo và `/exit` để thoát. JShell chạy được khai báo class, method (hàm của Java) và lệnh trực tiếp, không cần khung main. Các comment `//` giải thích dòng code và kết quả cần quan sát.

## 2. Sealed Class

Sealed class giới hạn những class nào được kế thừa trực tiếp, giúp mô hình có số nhánh rõ ràng.

```java
sealed interface Result permits Success {} // Chỉ cho phép Success ở ví dụ này
final class Success implements Result {}
System.out.println(new Success() instanceof Result); // true
```

**Khi nào dùng**

- Mô hình có số nhánh được kiểm soát.
- Muốn giới hạn kiểu được kế thừa.

**Lỗi hay gặp**

- Thêm kiểu con nhưng quên khai báo quan hệ cho phép.

**Tự gõ, không dùng AI sinh code:** Vẽ hai kết quả thành công và thất bại trước khi thêm class thứ hai. **Gợi ý:** Thêm tên class mới vào permits và chọn final cho class lá.

## 3. Pattern Matching Cho `instanceof`

Pattern matching cho instanceof vừa kiểm tra kiểu object vừa đặt tên biến theo kiểu đã kiểm tra.

```java
Object value = "Java";
if (value instanceof String text) { // text chỉ có khi value là String
    System.out.println(text.length()); // 4
}
```

**Khi nào dùng**

- Nhận Object rồi xử lý theo kiểu.
- Muốn tránh kiểm kiểu rồi cast riêng.

**Lỗi hay gặp**

- Dùng biến pattern ngoài vùng compiler bảo đảm đã khớp.

**Tự gõ, không dùng AI sinh code:** Đổi value thành số rồi null và quan sát nhánh if. **Gợi ý:** Không cần ép kiểu String lần nữa.

## 5. Text Block

Text block là chuỗi nhiều dòng dùng ba dấu nháy kép để dễ đọc nội dung có xuống dòng.

```java
String message = """
    Hello
    Java
    """;
System.out.print(message); // Hai dòng Hello và Java
```

**Khi nào dùng**

- Viết dữ liệu mẫu nhiều dòng.
- Giữ nội dung văn bản dễ nhìn trong code.

**Lỗi hay gặp**

- Không để xuống dòng sau dấu mở.
- Không kiểm khoảng trắng cuối kết quả.

**Tự gõ, không dùng AI sinh code:** Viết text block có ba dòng rồi xem khoảng trắng đầu dòng. **Gợi ý:** Dòng sau dấu mở là nơi nội dung bắt đầu.

## 4. Try-With-Resources

Try-with-resources tự đóng tài nguyên có hỗ trợ close khi rời khối try, kể cả khi gặp lỗi.

```java
try (var reader = new java.io.StringReader("Java")) { // Tự đóng reader
    System.out.println((char) reader.read()); // J: ký tự đầu
}
```

**Khi nào dùng**

- Đọc file hoặc mở reader.
- Dùng connection cần đóng.

**Lỗi hay gặp**

- Quên đóng tài nguyên khi có lỗi.
- Dùng tài nguyên sau khi khối try kết thúc.

**Quy tắc ngón tay cái:** Mở tài nguyên cần đóng: ưu tiên try-with-resources.

**Tự gõ, không dùng AI sinh code:** Đọc thêm ký tự thứ hai trong khối try. **Gợi ý:** Không dùng reader sau khi rời khối.

## 5. Custom Exception

Custom exception là loại lỗi bạn đặt tên để bên gọi nhận ra tình huống nghiệp vụ cụ thể.

```java
class MissingTaskException extends RuntimeException {
    MissingTaskException(long id) { super("Missing task " + id); }
}
System.out.println(new MissingTaskException(7).getMessage()); // Missing task 7
```

**Khi nào dùng**

- Caller cần phân biệt một lỗi nghiệp vụ.
- Tên lỗi làm cách xử lý rõ hơn.

**Lỗi hay gặp**

- Tạo một class lỗi cho từng thông báo dù không cần phân biệt.

**Tự gõ, không dùng AI sinh code:** Viết method ném MissingTaskException khi id không tồn tại trong danh sách mẫu. **Gợi ý:** Chỉ tạo lỗi riêng nếu bên gọi cần phân biệt tình huống đó.

## Đọc thêm sau

Luồng đóng nhiều tài nguyên, suppressed exception (lỗi phụ khi đóng), pattern switch nhiều nhánh và generic sealed hierarchy học sau. [Java 21 language changes](https://docs.oracle.com/en/java/javase/21/language/java-language-changes.html).
