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
