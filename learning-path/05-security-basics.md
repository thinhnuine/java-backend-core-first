# Session, Kiểm Quyền Và Luồng Đăng Nhập

Học tuần 17–18 mới. Cần CRUD PostgreSQL, frontend gọi API và test HTTP. Mục tiêu là dùng hai tài khoản để chứng minh quyền dữ liệu; chưa cần đăng ký, quên mật khẩu hoặc OAuth.

## 1. Ý chính

Authentication xác định identity của người gọi. Authorization kiểm tra identity đó có quyền làm một việc trên resource cụ thể không. Server phải làm cả hai; việc FE ẩn nút hay lưu userId không chứng minh quyền của request.

## 2. Giải thích luồng và cách thực hành

```text
Nhập username/password → Spring Security xác thực
→ tạo/đổi session theo cấu hình → browser giữ cookie session
→ request tiếp gửi cookie → server lấy principal
→ service/repository kiểm resource thuộc principal → trả dữ liệu
```

Session là state ở server gắn với một mã nhận diện trong cookie. Cookie không chứa toàn bộ user object của app. Password được đối chiếu qua PasswordEncoder; encoded password trong DB không được trả trong DTO. Khi logout, server làm session không còn hợp lệ.

Thực hành theo hai bước:

**Bước 1 — Quan sát cơ chế:** thêm Spring Security dependency do phiên bản Boot của project quản lý. Dùng cấu hình form login để quan sát đăng nhập từ browser; bắt đầu bằng hai user dev theo mẫu [username/password authentication](https://docs.spring.io/spring-security/reference/servlet/authentication/passwords/index.html) và [form login](https://docs.spring.io/spring-security/reference/servlet/authentication/passwords/form.html). Xem Network để tìm cookie và request sau login. Chưa tự viết một login controller lưu session bằng tay.

**Bước 2 — Contract của SPA:** chốt endpoint login, cách gửi credentials, endpoint lấy CSRF token, GET /me, logout và schema lỗi. Form login thường nhận form-urlencoded và có redirect mặc định; fetch gửi JSON không tự phù hợp. Chọn cấu hình theo tài liệu đúng version, thay success/failure handlers và authentication entry point để response hợp contract. Giữ luồng xác thực/session do Spring Security quản lý; nếu chọn login controller custom thì phải hiểu lưu SecurityContext và session strategy trước.

Bảng contract của project cần có:

| Request | Kỳ vọng sau khi cấu hình API |
| --- | --- |
| GET /me chưa đăng nhập | 401 JSON theo contract, không redirect HTML |
| Login đúng | Cookie session; kết quả thành công có format FE biết đọc |
| Login sai | Thất bại rõ, không lộ mật khẩu hoặc chi tiết nhạy cảm |
| GET task với session hợp lệ | Chỉ dữ liệu user có quyền |
| POST/PATCH/DELETE dùng cookie nhưng thiếu CSRF token | Bị từ chối trước khi ghi dữ liệu |
| Logout hợp lệ | Session cũ không còn truy cập endpoint riêng tư |

Ví dụ snippet ownership, đặt trong service đã có repository/user identity, không phải chương trình độc lập:

```java
TaskEntity task = repository.findByIdAndOwnerId(taskId, authenticatedUserId)
    .orElseThrow(() -> new TaskNotFoundException(taskId));
```

findByIdAndOwnerId là query bạn định nghĩa; authenticatedUserId phải được ánh xạ từ principal đã xác thực, không lấy từ ownerId trong request. Nếu owner nằm trên project, query theo owner của project hoặc bảo đảm field owner task nhất quán; tên query phải khớp model thật.

## 3. Vì sao thiết kế như vậy

Mẫu sai về quyền:

```java
// Sai: body do client tự gửi không chứng minh người gọi là owner.
TaskEntity task = repository.findByIdAndOwnerId(taskId, request.ownerId())
    .orElseThrow(() -> new TaskNotFoundException(taskId));
```

B biết ownerId của A thì có thể gửi theo. Identity phải tới từ cơ chế xác thực đã được server tin cậy. Danh sách và totalElements phân trang cũng phải lọc quyền, không chỉ endpoint đọc một task.

## 4. Liên hệ với frontend

State “đã login” ở React giúp render UI, nhưng server quyết định session hợp lệ. Với fetch, 401/403 vẫn là HTTP response đã nhận; cần kiểm response.ok/status, không chỉ dựa vào catch dành cho lỗi mạng. Khi session hết hạn, FE đưa người dùng về luồng login phù hợp.

## 5. Khi nào dùng và không dùng

Session phù hợp ứng dụng browser nhỏ trong khóa. Khi deploy, ưu tiên cùng origin bằng proxy để đơn giản hóa luồng cookie. Nếu khác origin, cấu hình origin cụ thể và credentials đúng; CORS kiểm hành vi browser, không thay kiểm quyền ở server.

Giữ CSRF protection cho flow cookie session. Theo [Spring Security CSRF guide](https://docs.spring.io/spring-security/reference/servlet/exploits/csrf.html), SPA cần xử lý token theo cấu hình, kể cả lấy token mới sau login/logout khi cần. HttpOnly giúp JS không đọc cookie session; nó không tự ngăn CSRF. Secure dùng khi HTTPS; SameSite chọn theo topology thật.

## 6. Bẫy hay gặp

Tắt CSRF để “sửa” 403; tưởng mọi 403 là thiếu role; thêm Security rồi endpoint trả login HTML trong khi FE đợi JSON; nghĩ controller advice bắt tất cả lỗi security filter; kiểm quyền cho đọc nhưng quên update/delete; gửi ownerId từ FE như nguồn identity.

## 7. Thuật ngữ mới

Principal là identity đã xác thực; SecurityContext giữ thông tin xác thực cho luồng xử lý; ownership là quan hệ sở hữu resource; session cookie liên hệ browser với state server; CSRF token giúp server kiểm tra yêu cầu thay đổi dữ liệu trong mô hình cookie.

## 8. Bài tập nhỏ để tự làm

1. Tạo hai user A/B trong dev, mỗi user một project/task.
2. Test A đọc/sửa/xóa task mình; test B gọi thẳng API với ID task của A.
3. Test list và tổng số phần tử chỉ tính dữ liệu được phép.
4. Quan sát một 403 do thiếu CSRF, rồi sửa cách gửi token theo cấu hình; không thay bằng tắt bảo vệ.
5. Logout và gọi lại bằng session cũ; ghi cả response và việc DB không có thay đổi trái quyền.

Đạt khi bạn giải thích được identity lấy ở đâu và có HTTP test cho các case trên. Tiếp theo: [chạy ứng dụng và CI](06-running-and-ci.md), dùng [project chặng E](03-task-manager-project.md) làm checklist triển khai.
