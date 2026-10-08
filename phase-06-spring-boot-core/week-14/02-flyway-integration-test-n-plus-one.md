# Flyway, Integration Test Và N+1

## Flyway

Migration giúp schema có version.

File:

```text
src/main/resources/db/migration/V1__create_projects_and_tasks.sql
```

Ví dụ:

```sql
CREATE TABLE projects (
    id BIGSERIAL PRIMARY KEY,
    name TEXT NOT NULL
);

CREATE TABLE tasks (
    id BIGSERIAL PRIMARY KEY,
    project_id BIGINT NOT NULL REFERENCES projects(id),
    title TEXT NOT NULL,
    status TEXT NOT NULL
);
```

## Lazy Loading

Lazy loading trì hoãn query relationship đến khi truy cập.

Rủi ro:

- query ngoài transaction.
- serialize entity làm trigger lazy load.
- N+1 query.

## N+1

N+1 xảy ra khi:

1. Query list tasks.
2. Với mỗi task, query project riêng.

Cách xử lý tùy case:

- fetch join.
- entity graph.
- DTO projection.
- query riêng rõ ràng.

## Integration Test

Integration test verify controller + database + Spring context cho flow chính.

Tối thiểu test:

- create task.
- get task.
- update task.
- validation error.
- not found.

Có thể dùng H2 để thử test đơn giản, nhưng nghiệm thu migration/SQL/transaction của khóa phải dùng PostgreSQL riêng hoặc Testcontainers PostgreSQL. Không kết luận tương thích PostgreSQL chỉ từ test H2.

## Bài Tập Nhanh

Tạo note:

- Query nào trong API có nguy cơ N+1.
- Bạn chọn cách tránh nào.
- Vì sao không trả entity trực tiếp.

Integration test kiểm hai hay nhiều thành phần thật cùng nhau, không bắt buộc luôn bao tất cả controller + DB + context. Trong project này cần ít nhất một flow HTTP → DB và test rollback qua Spring bean. Test đặt @Transactional ở lớp test có thể tự rollback hoặc che ranh giới thật; xác minh dữ liệu sau khi transaction nghiệp vụ đã kết thúc bằng transaction/connection phù hợp.

N+1 là query đầu rồi nhiều query phụ; không nhất thiết đúng một query cho mọi task nếu persistence context đã có một số project. Khi fetch join collection cùng pagination, kiểm SQL và cảnh báo ORM: đừng mặc định phân trang vẫn ở DB. Đo bằng tập dữ liệu có nhiều quan hệ, đọc cả số query lẫn dữ liệu nhận được.

Cấu hình Flyway và dependency hỗ trợ PostgreSQL theo phiên bản Boot/Flyway bạn dùng. Cho Hibernate validate schema thay vì ddl-auto=update; một công cụ chịu trách nhiệm đổi schema.
