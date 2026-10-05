# JPA, Hibernate Và Transaction

## Entity

Entity là object được JPA quản lý và map với table.

```java
@Entity
@Table(name = "tasks")
public class TaskEntity {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String title;
}
```

Entity cần constructor no-arg cho JPA, có thể protected.

## Repository

Repository thường extend Spring Data JPA interface:

```java
public interface TaskRepository extends JpaRepository<TaskEntity, Long> {
}
```

## Entity Vs DTO

Không trả entity trực tiếp ra API khi app bắt đầu nghiêm túc hơn.

Lý do:

- tránh expose field nội bộ.
- tránh lazy loading bất ngờ khi serialize.
- giữ API contract ổn định.

## Transaction

Transaction boundary thường đặt ở service method.

```java
@Transactional
public TaskResponse updateTask(Long id, UpdateTaskRequest request) {
    TaskEntity task = taskRepository.findById(id)
        .orElseThrow(() -> new TaskNotFoundException(id));
    task.rename(request.title());
    return mapper.toResponse(task);
}
```

## Dirty Checking

Trong transaction, entity managed thay đổi field có thể được Hibernate flush xuống database khi transaction commit.

Điều này tiện, nhưng cần hiểu để tránh "sao không gọi save mà DB vẫn đổi".

## Relationship

Ví dụ:

- Project có nhiều Task.
- Task thuộc một Project.

Chú ý:

- owner side.
- cascade.
- fetch type.
- orphan removal nếu dùng.

## Bài Tập Nhanh

Chuyển Task API từ in-memory sang JPA:

- `TaskEntity`.
- `ProjectEntity`.
- `TaskJpaRepository`.
- `ProjectJpaRepository`.
- Service dùng transaction.
