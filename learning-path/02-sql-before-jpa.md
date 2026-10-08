# Học SQL Trước Khi Dùng JPA

**Học ở tuần 9–11 mới.** Cần API in-memory chạy được. Tuần 9 làm schema/CRUD, tuần 10 JOIN/transaction/index, tuần 11 JDBC. Chỉ chạy bài trên database thực hành riêng, ví dụ `task_lab`.

## 1. Ý chính

Database quan hệ lưu dữ liệu trong các bảng có ràng buộc. SQL là ngôn ngữ để truy vấn và thay đổi dữ liệu đó. JPA giúp thao tác qua Java object nhưng bạn vẫn cần hiểu SQL được chạy và dữ liệu được bảo vệ ra sao.

Cài PostgreSQL local hoặc chạy bằng container nếu đã biết Docker; dùng `psql` hoặc SQL console của IDE kết nối database `task_lab`. Khi chưa biết cài và kết nối, theo [PostgreSQL tutorial](https://www.postgresql.org/docs/current/tutorial.html), hoàn thành Getting Started trước khi gõ SQL bên dưới. Ghi host, port, database, user vào note; không ghi password vào Git.

## 2. Giải thích code

Chạy một lần trên database bài tập còn trống:

```sql
CREATE TABLE projects (
    id BIGINT GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    name VARCHAR(100) NOT NULL
);

CREATE TABLE tasks (
    id BIGINT GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    project_id BIGINT NOT NULL REFERENCES projects(id),
    title VARCHAR(100) NOT NULL CHECK (length(trim(title)) > 0),
    status VARCHAR(20) NOT NULL DEFAULT 'TODO'
        CHECK (status IN ('TODO', 'IN_PROGRESS', 'DONE')),
    created_at TIMESTAMPTZ NOT NULL DEFAULT CURRENT_TIMESTAMP
);

INSERT INTO projects (name) VALUES ('Học backend') RETURNING id;
```

`PRIMARY KEY` xác định duy nhất một dòng; `IDENTITY` sinh ID. `REFERENCES` ngăn task trỏ tới project không tồn tại. `NOT NULL` ngăn thiếu giá trị, nhưng riêng nó không chặn chuỗi rỗng, vì vậy title có thêm `CHECK`.

Nếu lệnh INSERT trên trả ID 1, dùng 1 bên dưới; nếu trả ID khác, thay bằng ID thực tế:

```sql
INSERT INTO tasks (project_id, title)
VALUES (1, 'Học SQL');

SELECT id, title, status
FROM tasks
WHERE project_id = 1
ORDER BY created_at DESC, id DESC
LIMIT 20 OFFSET 0;

SELECT t.id, t.title, p.name AS project_name
FROM tasks t
JOIN projects p ON p.id = t.project_id;

SELECT status, COUNT(*) AS total
FROM tasks
GROUP BY status;
```

`WHERE` lọc dòng; `ORDER BY` sắp xếp; ID là tiêu chí phụ để thứ tự ổn định khi thời gian bằng nhau. `JOIN` ghép các dòng theo quan hệ; `GROUP BY` gom nhóm để đếm. Với dữ liệu lớn, OFFSET sâu có thể tốn chi phí; bài đầu chỉ cần hiểu offset pagination.

Thử transaction với một task có thật, thay ID nếu cần:

```sql
BEGIN;
UPDATE tasks SET status = 'DONE' WHERE id = 1;
SELECT status FROM tasks WHERE id = 1;
ROLLBACK;
SELECT status FROM tasks WHERE id = 1;
```

Trong transaction thấy DONE; sau ROLLBACK trạng thái trở về trước UPDATE. Thử lại với COMMIT để xác nhận khác biệt. Transaction bao nhóm thay đổi thành một đơn vị commit/rollback; không tự giải quyết mọi kiểu concurrent update.

JDBC ở tuần 11: viết method sau trong một class, thêm các import `java.sql.Connection`, `java.sql.PreparedStatement`, `java.sql.ResultSet`, `java.sql.SQLException`, `java.util.Optional`. Đây là snippet; caller phải mở connection bằng JDBC driver PostgreSQL, truyền vào và đóng connection bằng try-with-resources.

```java
static Optional<String> findTitle(Connection connection, long id)
        throws SQLException {
    String sql = "SELECT title FROM tasks WHERE id = ?";
    try (PreparedStatement statement = connection.prepareStatement(sql)) {
        statement.setLong(1, id);
        try (ResultSet rows = statement.executeQuery()) {
            if (rows.next()) {
                return Optional.of(rows.getString("title"));
            }
            return Optional.empty();
        }
    }
}
```

`?` giữ chỗ cho giá trị, `setLong(1, id)` bind tham số đầu tiên. `rows.next()` đưa con trỏ tới dòng kết quả nếu có. `try (...)` đóng statement/result set cả khi có lỗi. Method không đóng connection do caller truyền vào: nơi mở tài nguyên cần quy định rõ trách nhiệm đóng.

## 3. Vì sao thiết kế như vậy

Ví dụ sai: chỉ kiểm tra project tồn tại trong Java nhưng không có foreign key. Một thao tác khác có thể xóa project giữa lúc kiểm tra và lưu; DB có thể nhận task mồ côi. Constraint bảo vệ dữ liệu ngay tại nơi lưu.

Một mẫu sai khác là ghép input vào SQL: `"SELECT ... WHERE title = '" + input + "'"`. Input có dấu nháy có thể làm sai câu lệnh hoặc thay đổi ý nghĩa. Bind giá trị bằng prepared statement; tên cột sắp xếp thì kiểm tra bằng danh sách cho phép, không coi chúng là value parameter.

## 4. Liên hệ với frontend

`filter/map` xử lý dữ liệu đã tải về bộ nhớ; WHERE/SELECT giúp chỉ lấy dữ liệu cần từ DB. Nếu có hàng triệu task, không tải hết về Java rồi mới lọc/phân trang như một mảng UI nhỏ.

## 5. Khi nào dùng và không dùng

Dùng constraint cho tính hợp lệ của dữ liệu, transaction cho nhiều thay đổi phải thành công/thất bại cùng nhau. Dùng index theo truy vấn thật; không thêm index cho mọi cột vì mỗi lần ghi cũng phải cập nhật index.

Thử index và đọc kế hoạch truy vấn:

```sql
CREATE INDEX idx_tasks_project_created
ON tasks (project_id, created_at DESC, id DESC);

EXPLAIN SELECT id, title FROM tasks
WHERE project_id = 1
ORDER BY created_at DESC, id DESC LIMIT 20;
```

Bảng nhỏ có thể vẫn dùng sequential scan; đây không phải bằng chứng index bị hỏng. EXPLAIN cho kế hoạch ước tính; EXPLAIN ANALYZE thật sự chạy câu lệnh, nên hiểu tác động trước khi dùng cho câu lệnh ghi.

## 6. Bẫy hay gặp

- UPDATE/DELETE thiếu WHERE tác động mọi dòng.
- Dùng `= NULL`; SQL dùng `IS NULL` khi kiểm tra null.
- Nghĩ ID tăng liên tục không có khoảng trống; sequence/identity có thể bỏ số.
- Nghĩ rollback Java object đồng nghĩa object tự về state cũ; rollback DB không tự sửa lại mọi object đang cầm.
- Dùng H2 để kết luận mọi SQL sẽ chạy giống PostgreSQL.

## 7. Thuật ngữ mới

**Schema** là cấu trúc dữ liệu; **constraint** là ràng buộc DB kiểm tra; **migration** là thay đổi schema có version; **transaction** là đơn vị commit/rollback; **index** là cấu trúc hỗ trợ tìm kiếm; **JDBC** là API Java giao tiếp database qua driver.

## 8. Bài tập nhỏ để tự làm

1. Tạo hai project, mỗi project ba task với status khác nhau. Lưu SQL vào file để chạy lại trên DB bài tập mới.
2. Thử title rỗng, status sai và project không tồn tại. Ghi constraint nào từ chối mỗi trường hợp.
3. Viết LEFT JOIN hiển thị cả project không có task; đếm task mỗi project, kiểm tra nhóm rỗng có kết quả 0.
4. Tạo hai thay đổi trong một transaction rồi rollback; kiểm tra cả hai đều không còn tác động.
5. Dùng JDBC đọc task tồn tại/không tồn tại; cấu hình credential qua môi trường và đóng connection.
6. Viết DELETE cho một task, kiểm tra số dòng bị tác động trước khi tiếp tục.

**Đạt khi:** tự viết được các câu SQL trên theo dữ liệu mới và giải thích được vì sao cần constraint dù Java đã validate.

Tiếp theo: [JPA và transaction](../phase-06-spring-boot-core/week-14/01-jpa-hibernate-transaction.md). Dùng [PostgreSQL JDBC documentation](https://jdbc.postgresql.org/documentation/use/) để thiết lập driver/connection cho bài JDBC.
