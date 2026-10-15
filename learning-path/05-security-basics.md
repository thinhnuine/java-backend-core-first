# Session, Kiểm Quyền Và Luồng Đăng Nhập

> **Làm rõ trước khi học:** Ví dụ có số userId cố định chỉ minh họa so quyền; không phải cơ chế đăng nhập an toàn. Session hợp lệ và nguồn principal phải do server xác thực.

Học một ví dụ rồi dừng để tự sửa đầu vào. Các heading giữ nguyên để đối chiếu bản trước.

**Cách chạy ví dụ:** dùng JDK 21 (bộ công cụ Java). Mở terminal, gõ `jshell`; gõ từng khối Java độc lập, dùng `/reset` trước khối tiếp theo và `/exit` để thoát. JShell chạy được khai báo class, method (hàm của Java) và lệnh trực tiếp, không cần khung main. Các comment `//` giải thích dòng code và kết quả cần quan sát.

## 1. Ý chính

Authentication xác định người gọi là ai; authorization kiểm tra người đó được làm gì, để dữ liệu không bị truy cập trái quyền.

## 2. Giải thích luồng và cách thực hành

```java
record Task(long ownerId, String title) {}
Task task = new Task(1, "Private task");
long authenticatedUserId = 2; // Chỉ mô phỏng danh tính server đã xác thực
boolean allowed = task.ownerId() == authenticatedUserId;
System.out.println(allowed); // false: không phải chủ task
```

## 3. Vì sao thiết kế như vậy

Owner là người sở hữu dữ liệu. Id để kiểm quyền phải đến từ kết quả xác thực của server, không lấy ownerId do client tự khai. **Quy tắc ngón tay cái:** biết id của task không đồng nghĩa có quyền đọc task.

## 4. Liên hệ với frontend

Ẩn nút trong React chỉ thay giao diện; API vẫn cần kiểm quyền khi được gọi trực tiếp.

## 5. Khi nào dùng và không dùng

**Khi nào dùng**

- Trước khi đọc/sửa/xóa dữ liệu người dùng.
- Khi lọc danh sách và đếm tổng số phần tử.

## 6. Bẫy hay gặp

**Lỗi hay gặp**

- Tin ownerId do FE gửi.
- Chỉ kiểm quyền GET mà quên sửa/xóa.
- Tắt CSRF để bỏ qua lỗi 403.

## 7. Thuật ngữ mới

**Session**: thông tin phiên ở server. **Cookie**: dữ liệu browser gửi kèm request theo quy tắc. **Principal**: danh tính đã xác thực. **CSRF**: tấn công khiến browser gửi yêu cầu ngoài ý muốn bằng cookie đăng nhập.

## 8. Bài tập nhỏ để tự làm

**Tự gõ, không dùng AI sinh code:** Tạo hai tài khoản A/B; kiểm B không đọc/sửa/xóa task của A qua API.

**Gợi ý:** Kiểm cả danh sách và tổng số phần tử, không chỉ một task. Ví dụ Java trên chỉ là điều kiện minh họa.

## Đọc thêm sau

### Đăng nhập với Spring Security

Dùng cơ chế password/session của thư viện, không tự viết hệ thống lưu mật khẩu. PasswordEncoder băm mật khẩu để đối chiếu; không trả mật khẩu đã băm ra API. Chốt login/logout/GET /me và response 401 (chưa xác thực), 403 (bị từ chối), hoặc 404 cho tài nguyên không muốn tiết lộ.

Giữ CSRF protection cho mô hình session cookie, lấy/gửi token theo cấu hình. CORS kiểm việc browser đọc response khác origin, không thay authorization. HttpOnly chặn JS đọc cookie, Secure yêu cầu HTTPS, SameSite kiểm khi nào cookie được gửi giữa các site.

[Spring Security password authentication](https://docs.spring.io/spring-security/reference/servlet/authentication/passwords/index.html), [CSRF](https://docs.spring.io/spring-security/reference/servlet/exploits/csrf.html). OAuth và cấu hình nâng cao học sau.
