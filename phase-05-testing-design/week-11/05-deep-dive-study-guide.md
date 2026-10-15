# Mentor Guide Tuần 11: Testing Với JUnit, AssertJ Và Mockito

Học một ví dụ rồi dừng để tự sửa đầu vào. Các heading giữ nguyên để đối chiếu bản trước.

**Cách chạy ví dụ:** dùng JDK 21 (bộ công cụ Java). Mở terminal, gõ `jshell`; gõ từng khối Java độc lập, dùng `/reset` trước khối tiếp theo và `/exit` để thoát. JShell chạy được khai báo class, method (hàm của Java) và lệnh trực tiếp, không cần khung main. Các comment `//` giải thích dòng code và kết quả cần quan sát.

## 1. Ý chính

Boundary case là đầu vào sát giới hạn cho phép, dễ lộ sai dấu so sánh.

## 2. Giải thích code

```java
boolean allowed(String title) { return title.length() <= 100; }
if (!allowed("a".repeat(100))) throw new AssertionError("100 should pass");
if (allowed("a".repeat(101))) throw new AssertionError("101 should fail");
System.out.println("PASS"); // Hai ranh giới đúng
```

## 3. Vì sao thiết kế như vậy

Giới hạn 100 cần kiểm cả 100 và 101, không chỉ một chuỗi ngắn. **Quy tắc ngón tay cái:** tự giải thích được output trước khi mở rộng bài.

## 4. Liên hệ với Frontend

Giống Vitest/Jest: điều kiện độ dài tối đa cần test sát ranh giới, không chỉ test input thường.

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

**Tự gõ, không dùng AI sinh code:** Thêm trường hợp rỗng và tự chốt quy tắc.

**Gợi ý:** Expected đến từ yêu cầu, không từ code.

## Đọc thêm sau

Mockito, test nhiều tham số và benchmark test. Đây là phần tự chọn. [Quay lại trang module](README.md).
