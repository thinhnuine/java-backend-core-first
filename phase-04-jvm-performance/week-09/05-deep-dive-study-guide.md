# Mentor Guide Tuần 9: JVM Memory, Class Loading, JIT Và GC

Học một ví dụ rồi dừng để tự sửa đầu vào. Các heading giữ nguyên để đối chiếu bản trước.

**Cách chạy ví dụ:** dùng JDK 21 (bộ công cụ Java). Mở terminal, gõ `jshell`; gõ từng khối Java độc lập, dùng `/reset` trước khối tiếp theo và `/exit` để thoát. JShell chạy được khai báo class, method (hàm của Java) và lệnh trực tiếp, không cần khung main. Các comment `//` giải thích dòng code và kết quả cần quan sát.

## 1. Ý chính

Reference là giá trị tham chiếu tới object; hai biến có thể cùng giữ một object.

## 2. Giải thích code

```java
var first = new java.util.ArrayList<String>();
var second = first; // Không sao chép list
second.add("Java");
System.out.println(first.size()); // 1
```

## 3. Vì sao thiết kế như vậy

Muốn hiểu dữ liệu bị giữ, lần theo reference thay vì nhìn tên biến. **Quy tắc ngón tay cái:** tự giải thích được output trước khi mở rộng bài.

## 4. Liên hệ với Frontend

Trong JS, const second = first với array cũng giữ cùng object. Gán biến không tự clone nội dung.

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

**Tự gõ, không dùng AI sinh code:** Vẽ first, second và list ở giữa.

**Gợi ý:** Một object có thể có nhiều mũi tên trỏ tới.

## Đọc thêm sau

JIT/GC tuning, benchmark và classloader internals. Đây là phần tự chọn. [Quay lại trang module](README.md).
