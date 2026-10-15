# REST API, Validation Và Error Handling

> **Làm rõ trước khi học:** Các ví dụ record/validation ở đây là Java thuần, không tự tạo endpoint. @Size kiểm chuỗi hiện tại, không tự trim. Lỗi Spring Security có thể xảy ra trước controller advice.

**Cách chạy ví dụ:** dùng JDK 21 (bộ công cụ Java). Mở terminal, gõ `jshell`; gõ từng khối Java độc lập, dùng `/reset` trước khối tiếp theo và `/exit` để thoát. JShell chạy được khai báo class, method (hàm của Java) và lệnh trực tiếp, không cần khung main. Các comment `//` giải thích dòng code và kết quả cần quan sát.

## Layer Gợi Ý

Layer là nhóm code theo vai trò: controller nhận/trả HTTP, service xử lý quy tắc, repository đọc/ghi dữ liệu.

**Quy tắc ngón tay cái:** Theo luồng request, mỗi phần có nhiệm vụ rõ.

**Tự làm:** Vẽ đường tạo task qua ba phần. **Gợi ý:** Chưa có database thì repository có thể lưu trong bộ nhớ.

## DTO

DTO là object mang dữ liệu đi vào hoặc đi ra một ranh giới, chẳng hạn API.

```java
record CreateTaskRequest(String title) {} // Chỉ mang dữ liệu đầu vào
record TaskResponse(long id, String title) {} // Dữ liệu trả về
var input = new CreateTaskRequest("Java");
System.out.println(new TaskResponse(1L, input.title())); // TaskResponse[id=1, title=Java]
```

**Khi nào dùng**

- Mang input của API.
- Chỉ trả dữ liệu công khai cho FE.

**Lỗi hay gặp**

- Nghĩ record tự kiểm dữ liệu.
- Cho DTO chứa mọi field nội bộ.

**Tự gõ, không dùng AI sinh code:** Tạo response chỉ có id và title, không có dữ liệu nội bộ. **Gợi ý:** Record không tự kiểm tra title.

## Controller

Controller là nơi đổi request HTTP thành lời gọi nghiệp vụ và đổi kết quả thành response.

**Quy tắc ngón tay cái:** Controller mỏng, quy tắc nghiệp vụ ở service.

**Tự làm:** Viết contract POST /tasks với input và output mẫu. **Gợi ý:** Contract là thỏa thuận dữ liệu/status giữa FE và BE.

## Validation

Validation là kiểm đầu vào có thỏa quy tắc trước khi sử dụng hay không.

```java
String validTitle(String title) {
    if (title == null || title.isBlank()) throw new IllegalArgumentException("title required");
    String cleaned = title.trim(); // Chuẩn hóa trước khi kiểm độ dài
    if (cleaned.length() > 100) throw new IllegalArgumentException("title too long");
    return cleaned;
}
System.out.println(validTitle(" Java ")); // Java
```

**Khi nào dùng**

- Chặn input sai trước nghiệp vụ.
- Giữ quy tắc nhất quán giữa các lối gọi.

**Lỗi hay gặp**

- Chỉ kiểm ở FE.
- Dùng @Size trên chuỗi thô rồi nghĩ đã kiểm sau trim.

**Tự gõ, không dùng AI sinh code:** Thử title trắng, null và hơn 100 ký tự. **Gợi ý:** Ghi rõ quy tắc độ dài sau trim.

## Global Error Handling

Global error handling là nơi thống nhất cách đổi các lỗi ứng dụng thành response.

**Quy tắc ngón tay cái:** Cùng loại lỗi, cùng cấu trúc phản hồi.

**Tự làm:** Chốt lỗi không tìm thấy task phải có status và code nào. **Gợi ý:** Không trả stack trace (chuỗi lời gọi dẫn tới lỗi) cho client.

## Error Response

Error response là dữ liệu lỗi để frontend hiển thị hoặc xử lý nhất quán.

```java
record ApiError(String code, String message) {}
var error = new ApiError("TASK_NOT_FOUND", "Task không tồn tại");
System.out.println(error.code()); // TASK_NOT_FOUND
```

**Khi nào dùng**

- FE cần phân biệt loại lỗi.
- Nhiều endpoint cần cùng dạng thông báo.

**Lỗi hay gặp**

- Đổi cấu trúc lỗi tùy endpoint.
- Trả thông tin lỗi nội bộ cho client.

**Tự gõ, không dùng AI sinh code:** Thiết kế lỗi title không hợp lệ có thông tin field. **Gợi ý:** Tách mã lỗi ổn định với thông báo có thể đổi.

## Bài Tập Nhanh

Chốt hợp đồng API trước khi gắn annotation.

- POST tạo trả 201, GET thành công trả 200.
- Title sai trả 400, id không có trả 404.
- Xóa thành công trả 204 không body.

**Gợi ý:** Gọi trực tiếp API sau khi triển khai, không chỉ bấm UI.

**Đạt khi:** FE phân biệt được lỗi dữ liệu và lỗi server. Tự gõ phần triển khai, không dùng AI sinh lời giải.

## Đọc thêm sau

Trong Spring: @Valid kích hoạt kiểm tra request theo cấu hình, @RestControllerAdvice gom xử lý lỗi controller; @NotBlank cần thư viện Validation. Thêm Location cho response tạo mới. Xem [Spring MVC validation](https://docs.spring.io/spring-framework/reference/web/webmvc/mvc-controller/ann-validation.html).
