# Mockito Và Parameterized Tests

**Cách chạy ví dụ:** dùng JDK 21 (bộ công cụ Java). Mở terminal, gõ `jshell`; gõ từng khối Java độc lập, dùng `/reset` trước khối tiếp theo và `/exit` để thoát. JShell chạy được khai báo class, method (hàm của Java) và lệnh trực tiếp, không cần khung main. Các comment `//` giải thích dòng code và kết quả cần quan sát.

## Parameterized Test

Parameterized test là chạy cùng một quy tắc kiểm tra với nhiều bộ dữ liệu.

```java
for (String input : java.util.List.of(" Java ", "Java", "Java ")) {
    if (!input.trim().equals("Java")) throw new AssertionError(input); // Kiểm từng case
}
System.out.println("PASS"); // Mô hình Java thuần, chưa dùng JUnit
```

**Khi nào dùng**

- Khi cần thao tác này trong bài thực hành.
- Khi đọc code có cùng cách viết.

**Lỗi hay gặp**

- Dùng trước khi hiểu kiểu đầu vào và kết quả.

**Tự gõ, không dùng AI sinh code:** Thêm một case cố ý không đạt. **Gợi ý:** Ghi input nào làm kiểm tra thất bại.

## Mockito

Mockito tạo mock (object thay thế có thể cấu hình hành vi) để kiểm code mà không gọi dependency thật.

**Quy tắc ngón tay cái:** Chỉ thay phần bên ngoài bài đang kiểm, không thay chính logic cần kiểm.

**Tự làm:** Tìm một dependency gửi email trong service. **Gợi ý:** Nếu chỉ có vài dòng xử lý dữ liệu, không cần mock.

## Ví Dụ Mockito

Trước Mockito, dùng fake là implementation nhỏ bạn tự viết để quan sát lời gọi.

```java
interface Sender { void send(String text); }
class FakeSender implements Sender {
    String last;
    public void send(String text) { last = text; } // Ghi lại thay vì gửi thật
}
FakeSender sender = new FakeSender(); sender.send("Hi");
System.out.println(sender.last); // Hi
```

**Khi nào dùng**

- Khi cần thao tác này trong bài thực hành.
- Khi đọc code có cùng cách viết.

**Lỗi hay gặp**

- Dùng trước khi hiểu kiểu đầu vào và kết quả.

**Tự gõ, không dùng AI sinh code:** Truyền FakeSender vào service của bạn và kiểm last. **Gợi ý:** Constructor nhận Sender; service không tự new sender khác.

## Mock Vs Fake

Fake tự thực hiện hành vi đơn giản; mock cấu hình kỳ vọng qua thư viện. Cả hai là cách thay một dependency trong test.

**Quy tắc ngón tay cái:** Chọn cách ít cấu hình mà vẫn chứng minh đúng hành vi.

**Tự làm:** So việc lưu message bằng fake với kiểm send được gọi bằng mock. **Gợi ý:** Không cần thực hiện cả hai cho mọi test.

## Bài Tập Nhanh

Kiểm service mà không gửi email thật.

- Tạo fake ghi lại message.
- Gọi service.
- Kiểm message theo yêu cầu.

**Gợi ý:** Thử service tạo message sai để chắc test bắt được.

**Đạt khi:** Không gọi mạng và không kiểm message với chính nó. Tự gõ phần triển khai, không dùng AI sinh lời giải.

## Đọc thêm sau

JUnit @ParameterizedTest, @ValueSource, @NullSource; Mockito verify và stubbing (cấu hình giá trị trả về) đọc sau. Method void không dùng thenReturn; chọn doThrow/doAnswer khi phù hợp. [Mockito documentation](https://javadoc.io/doc/org.mockito/mockito-core/latest/org/mockito/Mockito.html).
