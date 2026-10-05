# Deep Dive Tuần 14: JPA, Transaction, Flyway Và Integration Test

## Cách Học Tuần Này

JPA/Hibernate rất tiện nhưng dễ gây bug âm thầm nếu bạn chỉ copy annotation. Tuần này học đủ để build API nhỏ và biết những bẫy đầu tiên: transaction, lazy loading, N+1, migration, integration test.

## Entity

Entity là object được JPA quản lý và map với table.

Nó khác DTO:

- Entity phục vụ persistence.
- DTO phục vụ API contract.

Đừng trả entity trực tiếp ra API khi app bắt đầu nghiêm túc.

## Repository

Spring Data JPA tạo implementation runtime cho repository interface.

```java
public interface TaskRepository extends JpaRepository<TaskEntity, Long> {
}
```

Bạn không thấy class implementation vì Spring tạo proxy.

## Transaction

Transaction boundary thường đặt ở service method.

```java
@Transactional
public TaskResponse updateTask(...) {
}
```

Trong transaction, entity managed được Hibernate theo dõi. Bạn đổi field, Hibernate có thể flush khi commit. Đó là dirty checking.

## Lazy Loading

Lazy loading nghĩa là relationship chưa query ngay. Khi truy cập mới query.

Vấn đề:

- truy cập ngoài transaction có thể lỗi
- serialize entity có thể trigger query bất ngờ
- list nhiều item có thể gây N+1

## N+1

N+1:

```text
1 query lấy list task
N query lấy project cho từng task
```

Cách xử lý:

- fetch join
- entity graph
- DTO projection
- query riêng

Không có một cách đúng cho mọi case.

## Flyway

Flyway giúp schema có version.

Thay vì sửa DB bằng tay, bạn viết migration:

```text
V1__create_projects_and_tasks.sql
```

Khi app chạy, Flyway áp migration chưa chạy.

## Integration Test

Integration test kiểm tra nhiều mảnh chạy cùng nhau:

- controller
- Spring context
- validation
- database/repository
- transaction

Unit test nhanh và nhỏ. Integration test ít hơn nhưng bắt lỗi wiring/config tốt.

## Bài Tập Theo Bước

1. Thêm PostgreSQL/Flyway config.
2. Viết migration tạo table.
3. Tạo entity Project/Task.
4. Tạo repository.
5. Chuyển service sang repository thật.
6. Đặt `@Transactional`.
7. Viết integration test create/get/update.
8. Ghi note N+1 risk.

## Lỗi Thường Gặp

- Trả entity trực tiếp ra API.
- Không biết transaction nằm ở đâu.
- Lazy loading bị trigger lúc serialize.
- Migration sửa rồi force DB bằng tay.
- Integration test phụ thuộc data cũ.

## Câu Hỏi Tự Kiểm Tra

- Entity khác DTO ở đâu?
- Repository implementation đến từ đâu?
- Dirty checking là gì?
- Transaction boundary nằm ở method nào?
- Query nào có nguy cơ N+1?
- Migration có chạy lại được trên máy người khác không?
