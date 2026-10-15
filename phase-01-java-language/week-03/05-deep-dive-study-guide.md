# Mentor Guide Tuần 3: Modern Java, Exception Và Capstone

Học một ví dụ rồi dừng để tự sửa đầu vào. Các heading giữ nguyên để đối chiếu bản trước.

**Cách chạy ví dụ:** dùng JDK 21 (bộ công cụ Java). Mở terminal, gõ `jshell`; gõ từng khối Java độc lập, dùng `/reset` trước khối tiếp theo và `/exit` để thoát. JShell chạy được khai báo class, method (hàm của Java) và lệnh trực tiếp, không cần khung main. Các comment `//` giải thích dòng code và kết quả cần quan sát.

## 1. Ý chính

Record mang dữ liệu; exception báo việc tạo dữ liệu không hợp lệ.

## 2. Giải thích code

```java
record Summary(String title) {}
Summary summarize(String title) {
    if (title == null || title.isBlank()) throw new IllegalArgumentException("title required");
    return new Summary(title.trim()); // Tạo kết quả đã kiểm
}
System.out.println(summarize(" Java ").title()); // Java
```

## 3. Vì sao thiết kế như vậy

Kiểm dữ liệu ở ranh giới nhận input, không đợi code phía sau gặp null. **Quy tắc ngón tay cái:** tự giải thích được output trước khi mở rộng bài.

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

**Tự gõ, không dùng AI sinh code:** Thêm giới hạn title dài tối đa 100 sau trim.

**Gợi ý:** Thử 100 và 101 ký tự.

## Đọc thêm sau

Sealed class, parser JSON và tự viết thư viện. Đây là phần tự chọn. [Quay lại trang module](README.md).
