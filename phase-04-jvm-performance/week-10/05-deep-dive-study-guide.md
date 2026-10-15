# Mentor Guide Tuần 10: Tooling, Heap Dump, Reflection Và Proxy

Học một ví dụ rồi dừng để tự sửa đầu vào. Các heading giữ nguyên để đối chiếu bản trước.

**Cách chạy ví dụ:** dùng JDK 21 (bộ công cụ Java). Mở terminal, gõ `jshell`; gõ từng khối Java độc lập, dùng `/reset` trước khối tiếp theo và `/exit` để thoát. JShell chạy được khai báo class, method (hàm của Java) và lệnh trực tiếp, không cần khung main. Các comment `//` giải thích dòng code và kết quả cần quan sát.

## 1. Ý chính

Reflection đọc thông tin class trong lúc chương trình chạy.

## 2. Giải thích code

```java
Class<?> type = String.class;
System.out.println(type.getSimpleName()); // String
System.out.println(type.getMethod("length").getReturnType()); // int
```

## 3. Vì sao thiết kế như vậy

Thông tin kiểu khác với dữ liệu của một object cụ thể. **Quy tắc ngón tay cái:** tự giải thích được output trước khi mở rộng bài.

## 4. Liên hệ với Frontend

JS có typeof và thao tác đọc thuộc tính object. Java reflection đọc mô tả class/method theo kiểu Java, không thay thế cách truy cập trực tiếp thông thường.

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

**Tự gõ, không dùng AI sinh code:** Tìm method isEmpty và kiểu trả về.

**Gợi ý:** Chưa cần tự quét package hoặc tạo container.

## Đọc thêm sau

Heap dump sâu, scanner, dynamic proxy và tự mô phỏng Spring. Đây là phần tự chọn. [Quay lại trang module](README.md).
