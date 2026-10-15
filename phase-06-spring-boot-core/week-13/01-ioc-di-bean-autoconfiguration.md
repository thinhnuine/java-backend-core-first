# Spring Tạo Object Và Nối Dependency Như Thế Nào?

> **Làm rõ trước khi học:** Ví dụ chính là mô hình DI Java thuần, chưa khởi động server Spring. Spring bean không tự thread-safe; final không tự inject.

Học một ví dụ rồi dừng để tự sửa đầu vào. Các heading giữ nguyên để đối chiếu bản trước.

**Cách chạy ví dụ:** dùng JDK 21 (bộ công cụ Java). Mở terminal, gõ `jshell`; gõ từng khối Java độc lập, dùng `/reset` trước khối tiếp theo và `/exit` để thoát. JShell chạy được khai báo class, method (hàm của Java) và lệnh trực tiếp, không cần khung main. Các comment `//` giải thích dòng code và kết quả cần quan sát.

## 1. Ý chính

Dependency injection (DI) là đưa object cần dùng từ bên ngoài vào; Spring có container (bộ quản lý object) để tự tạo và nối chúng theo cấu hình.

## 2. Giải thích code

```java
class Greeting { String text() { return "Hello"; } }
class Screen {
    private final Greeting greeting;
    Screen(Greeting greeting) { this.greeting = greeting; } // Nhận dependency
    void show() { System.out.println(greeting.text()); }
}
new Screen(new Greeting()).show(); // Hello: DI bằng Java thuần
```

## 3. Vì sao thiết kế như vậy

Bean là object do Spring container quản lý; IoC là việc công cụ điều phối quá trình thay vì mọi thứ do code của bạn gọi trực tiếp. **Quy tắc ngón tay cái:** DI không cần Spring; Spring giúp thực hiện việc nối object.

## 4. Liên hệ với frontend

Gần truyền dependency qua props hoặc tham số hàm. Annotation là dấu gắn lên code để framework đọc, không phải hành động tự chạy.

## 5. Khi nào dùng và không dùng

**Khi nào dùng**

- Constructor nhận object bắt buộc để hoạt động.
- Để đổi implementation hoặc kiểm thử dễ hơn.

## 6. Bẫy hay gặp

**Lỗi hay gặp**

- Nghĩ final tự đưa dependency vào field.
- Lưu người dùng/request hiện tại trong field dùng chung.
- Tự new rồi mong Spring áp dụng mọi tính năng quản lý.

## 7. Thuật ngữ mới

**Auto-configuration**: cấu hình tự chọn theo thư viện, cấu hình và bean hiện có. **Singleton bean**: một object dùng chung trong một container, không phải một object duy nhất trên mọi máy.

## 8. Bài tập nhỏ để tự gõ lại

**Tự gõ, không dùng AI sinh code:** Thay Greeting bằng interface và đưa một cách chào khác vào Screen.

**Gợi ý:** Giữ Screen chỉ gọi khả năng đã khai báo.

## Đọc thêm sau

### Bắt đầu Spring sau khi hiểu ví dụ

Tạo project tại [Spring Initializr](https://start.spring.io): Maven, Java 21, Spring Web, Validation; giữ phiên bản và POM được sinh, ghi lại trong README. Đặt các class dưới package của application class. Theo [first application](https://docs.spring.io/spring-boot/tutorial/first-application/index.html), tạo GET /hello, chạy `./mvnw spring-boot:run`, gọi `curl -i http://localhost:8080/hello`; cần thấy 200 và body bạn chọn.

@Service đánh dấu class nghiệp vụ; @RestController nhận/trả HTTP; @GetMapping gắn method với GET/path; constructor cho Spring thấy dependency. Component scan là tìm các class cần quản lý trong phạm vi package. Bean lifecycle, @Bean, @Qualifier, @Primary và chi tiết auto-configuration học sau.
