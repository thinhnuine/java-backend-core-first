# Unit Test Đầu Tiên Với Maven Và JUnit

> **Làm rõ trước khi học:** Ví dụ Java thuần không thay việc cài JUnit/Maven khi muốn test runner tự tìm và báo cáo test.

Học một ví dụ rồi dừng để tự sửa đầu vào. Các heading giữ nguyên để đối chiếu bản trước.

**Cách chạy ví dụ:** dùng JDK 21 (bộ công cụ Java). Mở terminal, gõ `jshell`; gõ từng khối Java độc lập, dùng `/reset` trước khối tiếp theo và `/exit` để thoát. JShell chạy được khai báo class, method (hàm của Java) và lệnh trực tiếp, không cần khung main. Các comment `//` giải thích dòng code và kết quả cần quan sát.

## 1. Ý chính

Unit test là kiểm tra nhỏ tự động gọi code và so kết quả với quy tắc bạn mong muốn, để phát hiện lỗi khi sửa bài.

## 2. Giải thích code

```java
String clean(String title) { return title.trim(); }
String actual = clean(" Java "); // Thực hiện hành vi
if (!"Java".equals(actual)) throw new AssertionError("must trim"); // Điều kiện kiểm tra
System.out.println("PASS"); // Điều kiện đúng
```

## 3. Vì sao thiết kế như vậy

Đây là assertion (điều kiện kiểm tra) bằng Java thuần. Nếu bỏ trim, chương trình phải báo lỗi thay vì PASS. **Quy tắc ngón tay cái:** tự làm hỏng hành vi một lần để biết test có bảo vệ nó.

## 4. Liên hệ với frontend

Giống expect trong Jest/Vitest về mục đích. JUnit là thư viện Java giúp tìm và chạy nhiều test, còn ví dụ này chỉ giúp hiểu một kiểm tra.

## 5. Khi nào dùng và không dùng

**Khi nào dùng**

- Kiểm tra tính toán và chuẩn hóa dữ liệu.
- Giữ một lỗi đã sửa không quay lại.

## 6. Bẫy hay gặp

**Lỗi hay gặp**

- So kết quả với chính nó.
- Chỉ kiểm đầu vào đúng.
- Thấy build thành công nhưng không có test chạy.

## 7. Thuật ngữ mới

**Expected**: kết quả mong đợi. **Actual**: kết quả thực tế. **Test runner**: công cụ tìm/chạy test. **Regression**: hành vi từng đúng bị hỏng lại.

## 8. Bài tập nhỏ để tự gõ lại

**Tự gõ, không dùng AI sinh code:** Tự viết kiểm tra cho chuỗi đã sạch và chuỗi có khoảng trắng hai đầu.

**Gợi ý:** Ghi expected độc lập; sau đó thêm trường hợp null theo quy tắc bạn chọn.

## Đọc thêm sau

### Chuyển sang JUnit khi hiểu kiểm tra

Dùng Maven project JDK 21. Production code ở src/main/java, test ở src/test/java. Thêm junit-jupiter ở test scope và maven-surefire-plugin theo [JUnit Maven setup](https://docs.junit.org/5.11.4/user-guide/#running-tests-build-maven); dùng @Test để đánh dấu method kiểm tra và assertEquals để so expected/actual. Với Spring project, giữ bộ phiên bản do Boot quản lý.

Chạy mvn test, xem số test và target/surefire-reports; “No tests to run” không phải bằng chứng test đã qua. Lần đầu cần tải thư viện. Mock (object giả có cấu hình), coverage (phần code được test chạy qua) và integration test (kiểm các phần thật phối hợp) học sau.
