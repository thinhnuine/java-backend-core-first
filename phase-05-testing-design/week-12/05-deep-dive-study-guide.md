# Mentor Guide Tuần 12: SOLID, Pattern Và Refactoring

Học một ví dụ rồi dừng để tự sửa đầu vào. Các heading giữ nguyên để đối chiếu bản trước.

**Cách chạy ví dụ:** dùng JDK 21 (bộ công cụ Java). Mở terminal, gõ `jshell`; gõ từng khối Java độc lập, dùng `/reset` trước khối tiếp theo và `/exit` để thoát. JShell chạy được khai báo class, method (hàm của Java) và lệnh trực tiếp, không cần khung main. Các comment `//` giải thích dòng code và kết quả cần quan sát.

## 1. Ý chính

Refactor là đổi cách tổ chức code mà không đổi kết quả đã cam kết.

## 2. Giải thích code

```java
String normalize(String title) { return title.trim(); }
String input = " Java ";
if (!normalize(input).equals("Java")) throw new AssertionError("changed behavior");
System.out.println("PASS");
```

## 3. Vì sao thiết kế như vậy

Tách method là hữu ích nếu tên giúp hiểu ý định hoặc dùng lại quy tắc. **Quy tắc ngón tay cái:** tự giải thích được output trước khi mở rộng bài.

## 4. Liên hệ với Frontend

Nếu đã có cách tương tự trong JS/TS, so theo hành vi và kiểu dữ liệu; không suy ra cơ chế chạy giống nhau.

## 5. Khi nào dùng và không dùng

**Khi nào dùng**

- Khi cần củng cố sau bài chính.
- Khi gặp đúng tình huống ví dụ trong code của bạn.

## 6. Bẫy hay gặp

**Lỗi hay gặp**

- Chép output mà chưa tự chạy.
- Mở rộng nhiều khái niệm cùng lúc.

## 7. Thuật ngữ mới

Các tên API trong ví dụ chỉ phục vụ một thao tác đang học. Nếu chưa biết class/method, quay lại bài cơ bản được dẫn trong README module.

## 8. Bài tập nhỏ để tự gõ lại

**Tự gõ, không dùng AI sinh code:** Tách một đoạn normalize đang bị lặp trong bài của bạn.

**Gợi ý:** Chạy kiểm tra trước và sau thay đổi.

## Đọc thêm sau

Áp mọi design pattern, abstraction nhiều tầng và kiến trúc lớn. Đây là phần tự chọn. [Quay lại trang module](README.md).
