# Final Project: Task API Hoặc Notes API

## Mục Tiêu

Hoàn thiện REST API nhỏ nhưng đủ chất backend: database, migration, validation, transaction, error handling và integration test.

## Yêu Cầu Tối Thiểu

- CRUD.
- PostgreSQL.
- Flyway migration.
- Validation.
- Global error response.
- Một relationship đơn giản, ví dụ task thuộc project.
- Transaction ở service layer.
- Integration test cho create/get/update.
- README setup local.

## Domain Gợi Ý: Task API

Project:

- id.
- name.
- createdAt.

Task:

- id.
- projectId.
- title.
- description.
- status.
- dueDate.
- createdAt.
- updatedAt.

## Endpoint Gợi Ý

- `POST /projects`.
- `GET /projects`.
- `POST /projects/{projectId}/tasks`.
- `GET /projects/{projectId}/tasks`.
- `GET /tasks/{taskId}`.
- `PUT /tasks/{taskId}`.
- `DELETE /tasks/{taskId}`.

## README Cần Có

- Cách chạy app.
- Cách chạy test.
- API endpoint chính.
- Request/response mẫu.
- Error response mẫu.
- Spring Boot đã tự cấu hình gì.
- Transaction nằm ở đâu.
- Lazy loading/N+1 bạn đã xử lý hoặc tránh thế nào.

## Acceptance

- App chạy local.
- Migration tự chạy.
- API chính gọi được.
- Test pass.
- README đủ để người khác clone về chạy.
- Bạn giải thích được từng annotation chính đang dùng.

Đây là mốc API dùng DB ở tuần 12–15 mới, chưa phải nghiệm thu cuối toàn khóa. Sau mốc này tiếp tục FE integration, security, chạy demo và bài tốt nghiệp trong [Task Manager](../../learning-path/03-task-manager-project.md).
