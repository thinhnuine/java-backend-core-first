# JPA, Hibernate Và Transaction

Trước bài này, hoàn thành [SQL trước JPA](../../learning-path/02-sql-before-jpa.md): primary/foreign key, JOIN và COMMIT/ROLLBACK. Các code block dưới đây là snippet cần class/import/dependency tương ứng trong Spring project.

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

    protected TaskEntity() {
    }

    public TaskEntity(String title) {
        rename(title);
    }

    public void rename(String title) {
        if (title == null || title.isBlank()) {
            throw new IllegalArgumentException("title is required");
        }
        String normalized = title.trim();
        if (normalized.length() > 100) {
            throw new IllegalArgumentException("title is too long");
        }
        this.title = normalized;
    }
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

Trong cấu hình proxy mặc định, `@Transactional` có hiệu lực khi lời gọi đi qua Spring proxy. Tự `new` service hoặc gọi method annotated từ một method khác cùng object không tự tạo transaction mới qua proxy. Mặc định rollback với `RuntimeException`/`Error`; checked exception cần cấu hình rollback nếu nghiệp vụ yêu cầu. Đừng catch rồi nuốt lỗi và mặc nhiên kỳ vọng rollback.

Transaction cũng không mặc định ngăn mọi lost update; học isolation/optimistic locking khi có concurrent requests. Xem [hướng dẫn @Transactional](https://docs.spring.io/spring-framework/reference/data-access/transaction/declarative/annotations.html).

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

Entity phía trên chỉ minh họa id/title ở bước đầu, chưa khớp schema task/project hoàn chỉnh: trước khi chạy migration của bước quan hệ, phải thêm status và project mapping. Không chép entity tối giản rồi kỳ vọng ghi được bảng có project_id/status NOT NULL.

Ở quan hệ Task → Project, owner side là phía giữ foreign key. @ManyToOne mặc định EAGER; nếu chọn LAZY hãy chỉ rõ fetch và cách lấy dữ liệu cho DTO trong transaction. EAGER không bảo đảm hết N+1. Không bật cascade ALL/REMOVE lên project từ task nếu việc xóa task không được phép xóa project. Flush gửi SQL, commit hoàn tất transaction; save không luôn đồng nghĩa commit ngay.
