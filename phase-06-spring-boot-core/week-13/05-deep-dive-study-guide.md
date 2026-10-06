# Mentor Guide Tuần 13: Spring IoC/DI, Autoconfiguration Và REST API

## 1. Ý chính

Spring Boot giúp bạn build backend nhanh, nhưng bạn cần hiểu cốt lõi: IoC container tạo object, DI inject dependency, autoconfiguration tạo cấu hình mặc định, controller nhận HTTP request. Mục tiêu tuần này là build REST API in-memory và hiểu request đi qua controller, service, repository.

## 2. Giải thích code

Service:

```java
package dev.thinh.task;

import org.springframework.stereotype.Service;

@Service
public class TaskService {
    private final TaskRepository repository;

    public TaskService(TaskRepository repository) {
        this.repository = repository;
    }
}
```

Giải thích:

- `@Service`: annotation báo class này là service bean.
- `private final TaskRepository repository`: dependency.
- Constructor injection: Spring inject `TaskRepository` bean vào constructor.

Controller:

```java
@RestController
@RequestMapping("/tasks")
public class TaskController {
    private final TaskService service;

    public TaskController(TaskService service) {
        this.service = service;
    }
}
```

- `@RestController`: class xử lý HTTP request và trả response body.
- `@RequestMapping("/tasks")`: prefix endpoint.
- Controller phụ thuộc service, không tự xử lý business dài.

DTO:

```java
public record CreateTaskRequest(@NotBlank String title) {
}
```

- `record`: DTO ngắn gọn.
- `@NotBlank`: validation title không null/blank.

## 3. Vì sao thiết kế như vậy

Nếu controller chứa hết logic:

```java
@PostMapping
public TaskResponse create(@RequestBody CreateTaskRequest request) {
    // validate, create id, save map, handle error...
}
```

Controller sẽ phình to và khó test. Tách layer giúp:

- Controller lo HTTP.
- Service lo business.
- Repository lo lưu trữ.

Nếu service import `ResponseEntity`, service đã bị dính HTTP, sau này dùng service từ job/message consumer sẽ khó.

## 4. Liên hệ với Frontend

Controller giống API route trong Next.js:

```ts
export async function POST(req: Request) {}
```

Nhưng trong Spring, bạn thường tách rõ controller/service/repository hơn. DI của Spring hơi giống việc truyền dependency qua props/context, nhưng chạy ở backend container.

## 5. Khi nào dùng và không dùng

| Thành phần | Khi dùng | Khi tránh |
| --- | --- | --- |
| Controller | Mapping HTTP, DTO, status code | Business logic dài |
| Service | Business rule, transaction boundary | Trả `ResponseEntity` |
| Repository | Persistence | Validate HTTP request |
| DTO | API request/response | Nhồi entity ra ngoài |
| `@RestControllerAdvice` | Error response nhất quán | Throw stack trace ra client |

## 6. Bẫy hay gặp

- Field injection thay vì constructor injection.
- Controller chứa business logic.
- Trả entity trực tiếp.
- Không có validation.
- Error response mỗi chỗ một kiểu.
- Class nằm ngoài component scan path.

## 7. Thuật ngữ mới

- `IoC`: container quản lý object lifecycle.
- `DI`: dependency injection, đưa dependency từ ngoài vào.
- `bean`: object do Spring quản lý.
- `autoconfiguration`: Spring Boot tự cấu hình dựa trên dependency/config.
- `DTO`: object cho request/response API.
- `controller/service/repository`: các layer phổ biến trong backend.

## 8. Bài tập nhỏ để tự gõ lại

1. Tạo Task API in-memory.
2. Endpoint `POST /tasks`.
3. Endpoint `GET /tasks/{id}`.
4. DTO request/response.
5. Validation `@NotBlank`.
6. Global error handler cho not found.

Học tiếp theo: sau khi API chạy in-memory, tuần 14 mới đưa vào database/JPA.
