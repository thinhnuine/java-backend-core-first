# Generics: Biết Trong Hộp Có Kiểu Dữ Liệu Gì

> **Làm rõ trước khi học:** T không phải any. Generics kiểm tra kiểu, không tự từ chối null hay kiểm tra nghiệp vụ.

Học một ví dụ rồi dừng để tự sửa đầu vào. Các heading giữ nguyên để đối chiếu bản trước.

**Cách chạy ví dụ:** dùng JDK 21 (bộ công cụ Java). Mở terminal, gõ `jshell`; gõ từng khối Java độc lập, dùng `/reset` trước khối tiếp theo và `/exit` để thoát. JShell chạy được khai báo class, method (hàm của Java) và lệnh trực tiếp, không cần khung main. Các comment `//` giải thích dòng code và kết quả cần quan sát.

## 1. Ý chính

Generics cho bạn ghi kiểu dữ liệu được phép đưa vào một hộp hoặc danh sách, để Java phát hiện nhầm kiểu trước khi chạy.

## 2. Giải thích code

```java
class Box<T> { // T là tên đại diện cho kiểu do người dùng hộp chọn
    private final T value;
    Box(T value) { this.value = value; } // Nhận đúng kiểu T
    T get() { return value; } // Trả lại cùng kiểu đó
}
Box<String> title = new Box<>("Java"); // T là String
System.out.println(title.get().length()); // 4: kết quả get là String
```

## 3. Vì sao thiết kế như vậy

Không dùng generics, hộp có thể trả Object (kiểu chung của các object), khiến bạn phải cast: tự yêu cầu Java xem giá trị theo kiểu cụ thể. Cast sai có thể gây lỗi lúc chạy. **Quy tắc ngón tay cái:** đọc Box<String> là “hộp chứa String”.

## 4. Liên hệ với frontend

Giống `Box<string>` trong TS về việc liên kết kiểu đầu vào và đầu ra. Dấu `<>` bên phải cho Java suy ra kiểu; không có nghĩa là hộp chứa mọi kiểu.

## 5. Khi nào dùng và không dùng

**Khi nào dùng**

- Khi dùng List<String> (danh sách chuỗi) hoặc Map<Long, String> (tra chuỗi theo khóa long bọc trong Long).
- Khi một class cần làm cùng việc cho nhiều kiểu.

## 6. Bẫy hay gặp

**Lỗi hay gặp**

- Viết Box<int>; dùng Integer vì generics nhận kiểu object.
- Bỏ <String> và dùng Box trần, làm mất kiểm tra kiểu.

## 7. Thuật ngữ mới

**Type parameter**: tên đại diện như T. **Type argument**: kiểu được chọn như String. **Compiler**: công cụ kiểm tra và dịch code trước khi chạy.

## 8. Bài tập nhỏ để tự gõ lại

**Tự gõ, không dùng AI sinh code:** Tạo Box<Integer>, lấy số ra và cộng 1; sau đó thử đưa String vào hộp đó để xem lỗi.

**Gợi ý:** Giữ class Box; chỉ thay kiểu được chọn. Xóa dòng cố ý sai sau khi đọc lỗi.

## Đọc thêm sau

Wildcard (dấu ? đại diện kiểu chưa biết), PECS (quy tắc chọn chiều đọc/ghi) và type erasure (cách thông tin kiểu được xử lý khi chạy) nằm trong [tra cứu nâng cao](06-advanced-reference.md). Chưa cần chúng để dùng Box và List.
