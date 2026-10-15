# Mentor Guide Tuần 13: Spring IoC/DI, Autoconfiguration Và REST API

Học một ví dụ rồi dừng để tự sửa đầu vào. Các heading giữ nguyên để đối chiếu bản trước.

**Cách chạy ví dụ:** dùng JDK 21 (bộ công cụ Java). Mở terminal, gõ `jshell`; gõ từng khối Java độc lập, dùng `/reset` trước khối tiếp theo và `/exit` để thoát. JShell chạy được khai báo class, method (hàm của Java) và lệnh trực tiếp, không cần khung main. Các comment `//` giải thích dòng code và kết quả cần quan sát.

## 1. Ý chính

Biến cục bộ thuộc một lần gọi; field của service có thể được nhiều request dùng chung.

## 2. Giải thích code

```java
class GreetingService {
    String greet(String name) { return "Hi " + name; } // Không lưu name vào field
}
var service = new GreetingService();
System.out.println(service.greet("An")); // Hi An
System.out.println(service.greet("Binh")); // Hi Binh
```

## 3. Vì sao thiết kế như vậy

Service dùng chung không nên giữ currentUser hay input hiện tại trong field. **Quy tắc ngón tay cái:** tự giải thích được output trước khi mở rộng bài.

## 4. Liên hệ với Frontend

Trong một hàm JS, biến cục bộ của mỗi lời gọi khác với dữ liệu đặt ở module dùng chung. Hãy hỏi ai có thể giữ và sửa dữ liệu đó.

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

**Tự gõ, không dùng AI sinh code:** Kiểm TaskService có field nào chỉ phục vụ một request.

**Gợi ý:** Chuyển dữ liệu đó thành tham số hoặc biến cục bộ khi phù hợp.

## Đọc thêm sau

Bean lifecycle, auto-configuration sâu và nhiều cấu hình bean. Đây là phần tự chọn. [Quay lại trang module](README.md).
