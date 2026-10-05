# Project Tuần 13: In-Memory Task API

## Mục Tiêu

Build REST API bằng Spring Boot nhưng persistence tạm thời in-memory để tập trung vào IoC/DI, REST layer, validation và error handling.

## Domain

Task:

- `id`.
- `title`.
- `description`.
- `status`: `TODO`, `IN_PROGRESS`, `DONE`.
- `createdAt`.
- `updatedAt`.

## Endpoint

- `POST /tasks`.
- `GET /tasks/{id}`.
- `GET /tasks`.
- `PUT /tasks/{id}`.
- `DELETE /tasks/{id}`.

## Layer

- `TaskController`.
- `TaskService`.
- `TaskRepository` interface.
- `InMemoryTaskRepository`.
- DTO request/response.
- `ApiExceptionHandler`.

## Validation

- title required.
- title max length.
- status hợp lệ.
- id không tồn tại trả 404.

## Test

- Service unit test không cần Spring.
- Controller slice/integration test nếu kịp.
- Test validation error.
- Test not found.

## README Project

Cần có:

- cách chạy app.
- endpoint chính.
- request/response mẫu.
- error response mẫu.
- giải thích bean nào được Spring tạo.
- dependency nào kích hoạt web autoconfiguration.
