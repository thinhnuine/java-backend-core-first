# Học SQL Trước Khi Dùng JPA

> **Làm rõ trước khi học:** Ví dụ Java phía trên chỉ mô hình hóa một dòng dữ liệu; nó không lưu PostgreSQL. Các câu SQL bên dưới chạy trong database, không chạy trong JShell.

Học một ví dụ rồi dừng để tự sửa đầu vào. Các heading giữ nguyên để đối chiếu bản trước.

**Cách chạy ví dụ:** dùng JDK 21 (bộ công cụ Java). Mở terminal, gõ `jshell`; gõ từng khối Java độc lập, dùng `/reset` trước khối tiếp theo và `/exit` để thoát. JShell chạy được khai báo class, method (hàm của Java) và lệnh trực tiếp, không cần khung main. Các comment `//` giải thích dòng code và kết quả cần quan sát.

## 1. Ý chính

SQL là ngôn ngữ đọc/ghi database quan hệ (dữ liệu lưu theo bảng); học bảng và truy vấn giúp bạn hiểu dữ liệu phía server.

## 2. Giải thích code

```java
record TaskRow(long id, String title) {} // Mô hình một dòng, chưa dùng database
var rows = java.util.List.of(new TaskRow(1, "Java"), new TaskRow(2, "SQL"));
for (TaskRow row : rows) { // Mô phỏng chọn dòng id = 2
    if (row.id() == 2) System.out.println(row.title()); // SQL
}
```

## 3. Vì sao thiết kế như vậy

Trong database, câu SELECT chọn dữ liệu thay cho vòng lặp minh họa. **Quy tắc ngón tay cái:** viết được SQL tương ứng trước khi để thư viện sinh SQL hộ.

## 4. Liên hệ với frontend

Array object ở FE có thể giống các dòng để hình dung, nhưng database lưu lâu dài và có ràng buộc riêng; một mảng trong RAM không thay database.

## 5. Khi nào dùng và không dùng

**Khi nào dùng**

- Lưu dữ liệu còn tồn tại sau restart.
- Tìm và kết hợp dữ liệu giữa các bảng.

## 6. Bẫy hay gặp

**Lỗi hay gặp**

- Ghép dữ liệu người dùng trực tiếp vào chuỗi SQL.
- Nghĩ xóa khỏi mảng Java đã xóa DB.
- Thử thay đổi schema trên database không phải bài tập.

## 7. Thuật ngữ mới

**Table/row/column**: bảng/dòng/cột. **Primary key**: khóa phân biệt từng dòng. **Foreign key**: khóa liên kết tới dòng ở bảng khác. **Constraint**: quy tắc DB bắt buộc dữ liệu tuân theo.

## 8. Bài tập nhỏ để tự làm

**Tự gõ, không dùng AI sinh code:** Trong database task_lab riêng, tạo bảng tasks có id và title, thêm hai dòng rồi SELECT theo id.

**Gợi ý:** Dùng SQL console của IDE hoặc psql (công cụ dòng lệnh PostgreSQL); làm Getting Started ở nguồn dưới trước khi chạy SQL.

## Đọc thêm sau

### Thực hành SQL từng bước

```sql
CREATE TABLE tasks (id BIGINT PRIMARY KEY, title TEXT NOT NULL);
INSERT INTO tasks VALUES (1, 'Java');
SELECT id, title FROM tasks WHERE id = 1;
UPDATE tasks SET title = 'SQL' WHERE id = 1;
DELETE FROM tasks WHERE id = 1;
```

Chỉ dùng bảng bài tập mới. Mỗi câu chạy riêng và đọc kết quả: tạo bảng → thêm → đọc → sửa → xóa.

### Học sau khi CRUD chạy được

CRUD là tạo/đọc/sửa/xóa. JOIN ghép bảng, GROUP BY gom dòng để tính, index là cấu trúc hỗ trợ tìm kiếm. Transaction nhóm thay đổi; BEGIN bắt đầu, COMMIT xác nhận, ROLLBACK hủy. JDBC là API Java gọi database; PreparedStatement giữ câu SQL và giá trị tham số tách riêng, tránh ghép input vào SQL. Tự làm một SELECT tham số hóa và đóng connection bằng try-with-resources trước JPA.

[PostgreSQL tutorial](https://www.postgresql.org/docs/current/tutorial.html), [JDBC basics](https://docs.oracle.com/javase/tutorial/jdbc/basics/).
