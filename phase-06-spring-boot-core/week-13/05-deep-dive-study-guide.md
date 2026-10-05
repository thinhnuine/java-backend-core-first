# Deep Dive Tuần 13: Spring IoC/DI, Autoconfiguration Và REST API

## Cách Học Tuần Này

Bạn đã học Java core trước để khi vào Spring không thấy toàn annotation "ma thuật". Tuần này mục tiêu không phải thuộc hết Spring, mà là hiểu request đi qua layer nào và bean được tạo/inject ra sao.

## IoC

Inversion of Control nghĩa là bạn không tự quản lý vòng đời object chính nữa. Spring container tạo object, nối dependency, cấu hình lifecycle.

Code không Spring:

```java
TaskRepository repo = new InMemoryTaskRepository();
TaskService service = new TaskService(repo);
```

Code Spring:

```java
@Service
public class TaskService {
    public TaskService(TaskRepository repository) {
    }
}
```

Spring tìm bean `TaskRepository` và inject vào.

## Constructor Injection

Ưu tiên constructor injection vì:

- dependency rõ ràng
- dễ test không cần Spring
- field có thể `final`
- object không ở trạng thái thiếu dependency

Tránh field injection trong code học nghiêm túc.

## Component Scanning

Spring scan từ package chứa application class xuống dưới. Nếu class nằm ngoài package tree đó, annotation có thể không được scan.

Đây là lỗi rất thường gặp khi mới học Spring.

## Autoconfiguration

Spring Boot nhìn classpath và config để tạo bean mặc định.

Ví dụ:

- có web starter -> cấu hình web server/MVC
- có validation -> enable validation
- có JPA + datasource -> entity manager/repository infrastructure

Autoconfiguration không phải phép màu. Nó là conditional config.

## REST Layer

Controller nên làm:

- nhận HTTP request
- validate DTO
- gọi service
- trả response DTO/status

Service nên làm:

- business logic
- transaction boundary sau này
- không phụ thuộc HTTP

Repository nên làm:

- persistence

## DTO

DTO giúp API contract tách khỏi domain/entity.

Request DTO có validation:

```java
public record CreateTaskRequest(@NotBlank String title) {
}
```

Response DTO chỉ chứa field muốn expose.

## Error Handling

`@RestControllerAdvice` giúp chuẩn hóa lỗi.

API tốt không trả stack trace lung tung cho client. Nó trả code/message rõ.

## Bài Tập Theo Bước

1. Tạo Spring Boot app.
2. Tạo `TaskController`.
3. Tạo `TaskService`.
4. Tạo `TaskRepository` interface.
5. Implement in-memory repository.
6. Thêm DTO.
7. Thêm validation.
8. Thêm global error handler.

## Lỗi Thường Gặp

- Controller chứa business logic.
- Service import HTTP class như `ResponseEntity`.
- DTO và entity/domain trộn lẫn.
- Field injection.
- Không có error response nhất quán.

## Câu Hỏi Tự Kiểm Tra

- Bean nào do Spring tạo?
- Dependency nào được inject ở đâu?
- Controller có logic dài không?
- Service có biết HTTP không?
- Validation lỗi trả response gì?
