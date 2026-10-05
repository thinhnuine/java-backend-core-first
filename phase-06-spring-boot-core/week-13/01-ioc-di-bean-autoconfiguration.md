# IoC, DI, Bean Lifecycle Và Autoconfiguration

## IoC

IoC nghĩa là lifecycle object do container quản lý.

Trong app Java thường, bạn tự:

```java
TaskService service = new TaskService(repository);
```

Trong Spring, container tạo bean và inject dependency.

## Dependency Injection

Ưu tiên constructor injection:

```java
@Service
public class TaskService {
    private final TaskRepository taskRepository;

    public TaskService(TaskRepository taskRepository) {
        this.taskRepository = taskRepository;
    }
}
```

Lợi ích:

- dependency rõ.
- dễ test.
- field có thể `final`.
- tránh object thiếu dependency.

## Bean

Bean là object do Spring container quản lý.

Bean có thể đến từ:

- `@Component`, `@Service`, `@Repository`, `@Controller`.
- `@Bean` method trong configuration.
- autoconfiguration.

## Component Scanning

Spring scan package từ application class trở xuống để tìm stereotype annotation.

Nếu class nằm ngoài scan path, bean không được tạo.

## Bean Lifecycle Tối Giản

1. Instantiate.
2. Inject dependency.
3. Run aware/callback/post processor.
4. Ready.
5. Destroy callback khi context đóng.

Bạn chưa cần thuộc hết hook, nhưng cần hiểu object không còn do bạn tự `new` trong app code chính.

## Autoconfiguration

Spring Boot nhìn dependency và config để tự cấu hình bean mặc định.

Ví dụ:

- Có `spring-boot-starter-web` thì cấu hình web server và MVC.
- Có JPA starter và datasource config thì cấu hình EntityManager/DataSource.

## Bài Tập Nhanh

Tạo Spring Boot app nhỏ:

- `TaskService`.
- `InMemoryTaskRepository`.
- Inject repository vào service.
- Viết test service bằng constructor bình thường, không cần Spring context.
