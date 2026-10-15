# Reflection, Annotation Và Dynamic Proxy

**Cách chạy ví dụ:** dùng JDK 21 (bộ công cụ Java). Mở terminal, gõ `jshell`; gõ từng khối Java độc lập, dùng `/reset` trước khối tiếp theo và `/exit` để thoát. JShell chạy được khai báo class, method (hàm của Java) và lệnh trực tiếp, không cần khung main. Các comment `//` giải thích dòng code và kết quả cần quan sát.

## Reflection

Reflection là đọc thông tin class hoặc gọi method qua thông tin đó trong lúc chạy.

```java
Class<?> type = String.class; // Object mô tả kiểu String
var method = type.getMethod("length"); // Tìm method theo tên
System.out.println(method.invoke("Java")); // 4: gọi qua reflection
```

**Khi nào dùng**

- Khi cần thao tác này trong bài thực hành.
- Khi đọc code có cùng cách viết.

**Lỗi hay gặp**

- Dùng trước khi hiểu kiểu đầu vào và kết quả.

**Quy tắc ngón tay cái:** Gọi trực tiếp khi biết kiểu; reflection khi thực sự cần đọc kiểu lúc chạy.

**Tự gõ, không dùng AI sinh code:** Tìm method isEmpty rồi gọi trên chuỗi rỗng. **Gợi ý:** Dùng getMethod với tên chính xác.

## Annotation

Annotation là dấu gắn lên code để công cụ hoặc chương trình đọc, không tự thực hiện nghiệp vụ.

```java
@Deprecated // Đánh dấu kiểu không còn nên dùng trong code mới
class OldTask {}
System.out.println(OldTask.class.isAnnotationPresent(Deprecated.class)); // true
```

**Khi nào dùng**

- Khi cần thao tác này trong bài thực hành.
- Khi đọc code có cùng cách viết.

**Lỗi hay gặp**

- Dùng trước khi hiểu kiểu đầu vào và kết quả.

**Tự gõ, không dùng AI sinh code:** Bỏ annotation rồi chạy phép kiểm tra. **Gợi ý:** Annotation chỉ hữu ích khi có nơi đọc và xử lý nó.

## Mini Scanner

Scanner là công cụ tìm các class hoặc dấu annotation theo một phạm vi.

**Quy tắc ngón tay cái:** Quét đúng phạm vi trước khi tự tạo object.

**Tự làm:** Liệt kê hai class bạn muốn quản lý và cách tìm chúng. **Gợi ý:** Lượt đầu dùng danh sách Class có sẵn, chưa quét cả ổ đĩa.

## Dynamic Proxy

Dynamic proxy là object trung gian được tạo lúc chạy để nhận lời gọi rồi chuyển tiếp cho object thật.

**Quy tắc ngón tay cái:** Proxy chỉ thấy lời gọi đi qua nó.

**Tự làm:** Vẽ caller → proxy ghi log → service. **Gợi ý:** Một method tự gọi method khác trong cùng object có thể không đi qua proxy.

## Liên Hệ Spring

Spring dùng nhiều cơ chế, trong đó có đọc annotation và proxy; nó không chỉ là vài lời gọi reflection.

**Quy tắc ngón tay cái:** Hiểu constructor injection trước khi mô phỏng framework.

**Tự làm:** Chỉ ra object nào do Spring tạo ở ứng dụng của bạn. **Gợi ý:** So với new trực tiếp; đừng kết luận mọi annotation tự có hiệu lực.

## Bài Tập Nhanh

Thử reflection ở phạm vi nhỏ.

- Lấy class của String.
- Tìm một method không tham số.
- Gọi và in kết quả.

**Gợi ý:** Dùng length hoặc isEmpty; sau đó so với gọi trực tiếp.

**Đạt khi:** Nêu được chi phí về độ dễ đọc và kiểm tra kiểu. Tự gõ phần triển khai, không dùng AI sinh lời giải.

## Đọc thêm sau

Tự viết annotation có retention (thời gian dấu được giữ), scanner và InvocationHandler cho proxy sau. Spring transaction còn có quản lý transaction thật; proxy đơn giản không tái hiện đầy đủ hành vi đó.
