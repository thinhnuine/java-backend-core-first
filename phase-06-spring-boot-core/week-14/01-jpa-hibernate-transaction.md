# JPA, Hibernate Và Transaction

> **Làm rõ trước khi học:** Record trong ví dụ chỉ minh họa DTO, không phải mẫu entity JPA. @Transactional không tự có hiệu lực trên object tự new hay mọi lời gọi bên trong cùng object.

**Cách chạy ví dụ:** dùng JDK 21 (bộ công cụ Java). Mở terminal, gõ `jshell`; gõ từng khối Java độc lập, dùng `/reset` trước khối tiếp theo và `/exit` để thoát. JShell chạy được khai báo class, method (hàm của Java) và lệnh trực tiếp, không cần khung main. Các comment `//` giải thích dòng code và kết quả cần quan sát.

## Entity

Entity là object được JPA (chuẩn Java ánh xạ dữ liệu) quản lý tương ứng với bản ghi database; Hibernate là công cụ thực hiện chuẩn đó.

**Quy tắc ngón tay cái:** Hiểu bảng trước khi ánh xạ thành entity.

**Tự làm:** Vẽ cột id và title của bảng tasks. **Gợi ý:** Entity cần khớp schema (cấu trúc bảng), không chỉ khớp giao diện.

## Repository

Repository là nơi gom thao tác đọc/ghi dữ liệu để service không tự xử lý từng truy vấn.

**Quy tắc ngón tay cái:** Nói rõ cần tìm gì, trả gì khi không có.

**Tự làm:** Viết bằng lời hợp đồng findById. **Gợi ý:** Kết quả thiếu có thể biểu diễn bằng Optional; đó chưa phải quyền đọc dữ liệu.

## Entity Vs DTO

Entity phục vụ lưu trữ; DTO phục vụ dữ liệu cần trả cho bên gọi, giúp không lộ mọi field nội bộ.

```java
record TaskData(long id, String title, String internalNote) {} // Dữ liệu mẫu
record TaskResponse(long id, String title) {}
var data = new TaskData(1, "Java", "private note");
var response = new TaskResponse(data.id(), data.title()); // Chỉ chọn field công khai
System.out.println(response); // Không có internalNote
```

**Khi nào dùng**

- Khi cần thao tác này trong bài thực hành.
- Khi đọc code có cùng cách viết.

**Lỗi hay gặp**

- Dùng trước khi hiểu kiểu đầu vào và kết quả.

**Tự gõ, không dùng AI sinh code:** Thêm một field nội bộ và giữ nguyên response. **Gợi ý:** Ví dụ không phải entity JPA; nó minh họa việc chọn dữ liệu.

## Transaction

Transaction là nhóm thao tác database cùng được xác nhận thành công hoặc hủy khi thất bại theo ranh giới đã chọn.

**Quy tắc ngón tay cái:** Một nghiệp vụ nhiều bước ghi cần xác định ranh giới transaction.

**Tự làm:** Vẽ tạo project rồi tạo task; nếu bước hai lỗi thì dữ liệu nào cần được hủy? **Gợi ý:** COMMIT xác nhận, ROLLBACK hủy các thay đổi chưa xác nhận.

## Dirty Checking

Dirty checking là Hibernate theo dõi thay đổi của entity đang được quản lý để gửi cập nhật xuống database.

**Quy tắc ngón tay cái:** Sửa object thường không đồng nghĩa dữ liệu đã commit.

**Tự làm:** Nêu khác biệt giữa sửa field, flush và commit. **Gợi ý:** Flush gửi SQL; commit hoàn tất transaction. Object ngoài quản lý không tự được lưu.

## Relationship

Relationship là quan hệ giữa các dữ liệu, như nhiều task thuộc một project.

**Quy tắc ngón tay cái:** Biết khóa liên kết nằm ở bảng nào trước khi thêm annotation.

**Tự làm:** Vẽ tasks.project_id trỏ tới projects.id. **Gợi ý:** Foreign key là ràng buộc khóa liên kết tới bản ghi ở bảng khác.

## Bài Tập Nhanh

Đổi cách lưu sau khi API và SQL đã hiểu.

- Ánh xạ Task vào bảng đã tạo.
- Lưu rồi đọc lại.
- Restart ứng dụng và xác nhận dữ liệu còn.

**Gợi ý:** Thêm từng field; không chép entity id/title vào bảng còn nhiều cột bắt buộc.

**Đạt khi:** Giải thích được SQL tương ứng với thao tác chính. Tự gõ phần triển khai, không dùng AI sinh lời giải.

## Đọc thêm sau

Entity cần constructor không tham số theo yêu cầu JPA. Trong cấu hình proxy mặc định, lời gọi phải qua Spring proxy; mặc định rollback cho RuntimeException/Error, checked exception cần cấu hình theo nghiệp vụ. Isolation (mức cô lập), optimistic locking (phát hiện cập nhật dựa trên version), cascade (lan thao tác sang quan hệ), lazy loading (tải khi cần) học từng phần sau. [Spring transaction reference](https://docs.spring.io/spring-framework/reference/data-access/transaction/declarative/annotations.html).
