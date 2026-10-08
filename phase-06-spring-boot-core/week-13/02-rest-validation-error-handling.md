# REST API, Validation Và Error Handling

## Layer Gợi Ý

- Controller: HTTP mapping, request/response DTO.
- Service: business logic, transaction boundary.
- Repository: persistence.

Controller không nên chứa business logic dài.

## DTO

Request DTO:

```java
public record CreateTaskRequest(
    @NotBlank String title,
    String description
) {
}
```

Response DTO:

```java
public record TaskResponse(
    Long id,
    String title,
    String status
) {
}
```

## Controller

```java
@RestController
@RequestMapping("/tasks")
public class TaskController {
    private final TaskService taskService;

    public TaskController(TaskService taskService) {
        this.taskService = taskService;
    }

    @PostMapping
    public ResponseEntity<TaskResponse> create(@Valid @RequestBody CreateTaskRequest request) {
        return ResponseEntity.status(HttpStatus.CREATED).body(taskService.create(request));
    }
}
```

## Validation

Annotation hay dùng:

- `@NotBlank`.
- `@NotNull`.
- `@Size`.
- `@Min`.
- `@Email`.

## Global Error Handling

Dùng `@RestControllerAdvice`:

```java
@RestControllerAdvice
public class ApiExceptionHandler {
    @ExceptionHandler(TaskNotFoundException.class)
    ResponseEntity<ApiError> handle(TaskNotFoundException ex) {
        return ResponseEntity.status(HttpStatus.NOT_FOUND)
            .body(new ApiError("TASK_NOT_FOUND", ex.getMessage()));
    }
}
```

## Error Response

Nên nhất quán:

```json
{
  "code": "TASK_NOT_FOUND",
  "message": "Task 1 was not found"
}
```

## Bài Tập Nhanh

Thiết kế API:

- `POST /tasks`.
- `GET /tasks/{id}`.
- `GET /tasks`.
- `PUT /tasks/{id}`.
- `DELETE /tasks/{id}`.

Ghi request/response/error mẫu vào README project.

Các snippet cần import jakarta.validation.Valid và jakarta.validation.constraints.*, org.springframework.web.bind.annotation.*, org.springframework.http.*; TaskService/ApiError/TaskNotFoundException phải do project định nghĩa. @NotBlank cần Validation dependency và @Valid trên request để kích hoạt kiểm tra trong flow này.

Service chuẩn hóa title rồi kiểm độ dài 1–100, vì rule của project là độ dài sau trim. @Size(max=100) kiểm raw String, có thể cho kết quả khác; chọn contract nhất quán và test case chứa khoảng trắng hai đầu. Handler mẫu chỉ xử lý not found; bổ sung lỗi validation, JSON sai/status sai và lỗi ngoài dự kiến theo cùng schema.

Nếu dùng Spring Security ở chặng sau, lỗi authentication/authorization/CSRF xảy ra ở filter chain có thể không đi qua controller advice; cấu hình error handling riêng ở tầng security. Khi tạo thành công, thêm Location tới resource vừa tạo như contract HTTP của project.
