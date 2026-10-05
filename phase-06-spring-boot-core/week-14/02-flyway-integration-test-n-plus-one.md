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

Nếu chưa dùng Testcontainers, có thể bắt đầu bằng H2/test database, nhưng ghi rõ khác biệt so với PostgreSQL.

## Bài Tập Nhanh

Tạo note:

- Query nào trong API có nguy cơ N+1.
- Bạn chọn cách tránh nào.
- Vì sao không trả entity trực tiếp.
