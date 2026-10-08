# Mentor Guide Tuần 14: JPA, Transaction, Flyway Và Integration Test

## 1. Ý chính

Tuần này đưa REST API vào database. JPA/Hibernate map entity với table, transaction bảo vệ một nhóm thay đổi, Flyway quản lý schema version, integration test kiểm tra app chạy qua Spring context và database. Đây là bước backend bắt đầu giống production hơn.

## 2. Giải thích code

Entity:

```java
package dev.thinh.task;

import jakarta.persistence.Entity;
import jakarta.persistence.GeneratedValue;
import jakarta.persistence.GenerationType;
import jakarta.persistence.Id;
import jakarta.persistence.Table;

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
        this.title = title;
    }
}
```

Giải thích:

- `@Entity`: class được JPA quản lý.
- `@Table(name = "tasks")`: map tới table `tasks`.
- `@Id`: primary key.
- `@GeneratedValue`: id do database sinh.
- `protected TaskEntity()`: constructor cho JPA dùng.

Repository:

```java
public interface TaskRepository extends JpaRepository<TaskEntity, Long> {
}
```

Spring Data JPA tạo implementation runtime.

Transaction:

```java
@Transactional
public TaskResponse updateTask(Long id, UpdateTaskRequest request) {
    TaskEntity task = repository.findById(id)
        .orElseThrow(() -> new TaskNotFoundException(id));
    task.rename(request.title());
    return mapper.toResponse(task);
}
```

`@Transactional` áp dụng transaction khi lời gọi đi qua proxy trong cấu hình mặc định; có thể tạo hoặc tham gia transaction. Commit/rollback theo propagation và rollback rules, không tự có hiệu lực khi new service hoặc self-invocation. Xem bài JPA chính cho các giới hạn.

## 3. Vì sao thiết kế như vậy

Không dùng migration, mỗi dev có thể tự sửa DB khác nhau. Flyway giúp schema có lịch sử:

```text
V1__create_tasks.sql
V2__add_due_date_to_tasks.sql
```

Không dùng transaction, update nhiều bảng có thể bị nửa thành công nửa thất bại.

Không để ý lazy loading, bạn dễ gặp N+1:

```text
1 query lấy tasks
N query lấy project cho từng task
```

## 4. Liên hệ với Frontend

Entity không giống React state hay DTO. Entity gắn với database và persistence context. DTO giống object bạn trả về frontend hơn.

Trong Next.js, migration có thể giống việc bạn version schema Prisma. Flyway trong Java đóng vai trò quản lý thay đổi DB bằng file SQL versioned.

## 5. Khi nào dùng và không dùng

| Công cụ | Khi dùng | Khi tránh |
| --- | --- | --- |
| Entity | Map database table | Trả trực tiếp ra API |
| DTO | API contract | Làm entity managed |
| `@Transactional` | Business operation cần atomic | Controller đơn giản không cần |
| Flyway | Quản lý schema | Sửa DB tay không version |
| Integration test | Kiểm controller+DB+Spring | Thay thế toàn bộ unit test |

## 6. Bẫy hay gặp

- Trả entity trực tiếp ra API.
- Không biết transaction nằm ở đâu.
- Lazy loading bị trigger khi serialize JSON.
- N+1 query.
- Migration sửa file đã chạy rồi.
- Test phụ thuộc data cũ trong database.

## 7. Thuật ngữ mới

- `entity`: object được JPA map với table.
- `persistence context`: vùng JPA quản lý entity đang tracked.
- `dirty checking`: Hibernate tự phát hiện entity thay đổi để flush.
- `transaction`: nhóm thao tác thành công/thất bại cùng nhau.
- `migration`: file thay đổi schema có version.
- `N+1`: pattern query dư thừa khi load relationship.

## 8. Bài tập nhỏ để tự gõ lại

1. Tạo migration `V1__create_projects_and_tasks.sql`.
2. Tạo `ProjectEntity`, `TaskEntity`.
3. Tạo repository.
4. Chuyển API từ in-memory sang JPA.
5. Đặt transaction ở service.
6. Viết integration test create/get/update.
7. Ghi note chỗ nào có nguy cơ N+1.

Học tiếp theo: sau final API, học Spring Security và Docker để project giống backend thực tế hơn.
