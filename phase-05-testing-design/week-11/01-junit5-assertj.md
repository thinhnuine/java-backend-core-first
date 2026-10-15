# JUnit 5 Và AssertJ

> **Làm rõ trước khi học:** Ví dụ dưới là kiểm tra bằng Java thuần để hiểu hành vi, không giả làm code JUnit/AssertJ. Thêm thư viện và test runner là bước riêng.

**Cách chạy ví dụ:** dùng JDK 21 (bộ công cụ Java). Mở terminal, gõ `jshell`; gõ từng khối Java độc lập, dùng `/reset` trước khối tiếp theo và `/exit` để thoát. JShell chạy được khai báo class, method (hàm của Java) và lệnh trực tiếp, không cần khung main. Các comment `//` giải thích dòng code và kết quả cần quan sát.

## JUnit 5 Basics

Unit test là chương trình nhỏ kiểm tra một hành vi; JUnit 5 là thư viện tổ chức và chạy những kiểm tra đó.

```java
String clean(String title) { return title.trim(); }
String actual = clean(" Java "); // Chạy hành vi cần kiểm tra
if (!"Java".equals(actual)) throw new AssertionError("title must be trimmed");
System.out.println("PASS"); // Chỉ tới đây nếu điều kiện đúng
```

**Khi nào dùng**

- Kiểm quy tắc dữ liệu nhanh.
- Bắt lỗi cũ quay lại sau sửa code.

**Lỗi hay gặp**

- Chỉ nhìn println mà không kiểm điều kiện.
- Nghĩ ví dụ này đã là test JUnit.

**Quy tắc ngón tay cái:** Test phải thất bại khi hành vi cần bảo vệ bị hỏng.

**Tự gõ, không dùng AI sinh code:** Bỏ trim và xem kiểm tra thất bại. **Gợi ý:** AssertionError ở đây là lỗi báo điều kiện kiểm tra không đúng; chưa dùng JUnit.

## Arrange, Act, Assert

Arrange, Act, Assert là ba bước: chuẩn bị dữ liệu, chạy hành vi, kiểm kết quả.

```java
String input = " SQL "; // Arrange: chuẩn bị
String result = input.trim(); // Act: thực hiện
if (!result.equals("SQL")) throw new AssertionError("wrong result"); // Assert
System.out.println("PASS");
```

**Khi nào dùng**

- Khi cần thao tác này trong bài thực hành.
- Khi đọc code có cùng cách viết.

**Lỗi hay gặp**

- Dùng trước khi hiểu kiểu đầu vào và kết quả.

**Tự gõ, không dùng AI sinh code:** Tách ba bước cho phép cộng hai số. **Gợi ý:** Expected là kết quả mong muốn từ yêu cầu, không lấy từ chính implementation.

## AssertJ

AssertJ là thư viện viết điều kiện kiểm tra theo chuỗi method dễ đọc.

**Quy tắc ngón tay cái:** Hiểu điều kiện cần kiểm trước khi chọn cú pháp thư viện.

**Tự làm:** Viết bằng lời: danh sách trả về phải có đúng hai phần tử theo thứ tự nào. **Gợi ý:** Lượt đầu JUnit assertions đủ dùng; AssertJ là tùy chọn.

## Test Behavior

Test behavior là kiểm hành vi bên ngoài quan sát được, thay vì bám vào cách code chia method bên trong.

**Quy tắc ngón tay cái:** Đổi cách triển khai nhưng giữ hành vi thì test nên vẫn qua.

**Tự làm:** Viết ba case title: hợp lệ, trắng, null. **Gợi ý:** Mỗi case nêu input và kết quả/lỗi mong muốn.

## Bài Tập Nhanh

Bắt đầu bằng một quy tắc của Task Manager.

- Viết kiểm tra title được trim.
- Cố ý làm sai code và thấy test thất bại.
- Khôi phục rồi chạy lại.

**Gợi ý:** Sau kiểm tra Java thuần, chuyển sang JUnit theo hướng dẫn cuối file.

**Đạt khi:** Không chấp nhận BUILD SUCCESS nếu chưa xác nhận test thực sự chạy. Tự gõ phần triển khai, không dùng AI sinh lời giải.

## Đọc thêm sau

Trong Maven project, đặt test dưới src/test/java, thêm JUnit Jupiter ở test scope (chỉ dùng cho test) và Surefire (công cụ Maven chạy unit test). Dùng [JUnit 5.11.4 guide](https://docs.junit.org/5.11.4/user-guide/) và [bài test đầu tiên](../../learning-path/04-first-unit-test.md). AssertJ và parameterized test học khi cần nhiều phép kiểm.
