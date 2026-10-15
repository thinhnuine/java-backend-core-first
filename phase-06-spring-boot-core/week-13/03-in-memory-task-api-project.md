# Project Tuần 13: In-Memory Task API

Đây là đề để tự làm, không phải đáp án. Chọn từng mục theo README module; không cần làm hết trong một buổi. Tự gõ code, dùng AI để xin gợi ý sau khi đã thử.

## Mục Tiêu

Tạo API lưu dữ liệu trong bộ nhớ để tập trung vào HTTP và quy tắc. Restart làm mất dữ liệu là giới hạn đã biết của chặng này, không phải lưu trữ lâu dài.

## Domain

Domain là dữ liệu/nghiệp vụ đang mô hình hóa. Task có id, title, description, status, createdAt, updatedAt. **Tự làm:** bắt đầu id/title/status. **Gợi ý:** thời điểm dùng Instant sau khi luồng tạo/lấy đúng.

## Endpoint

Endpoint là method+đường dẫn. Làm POST /tasks và GET /tasks/{id} trước, rồi GET danh sách, PUT sửa, DELETE xóa. **Tự làm:** ghi request/response. **Gợi ý:** tạo trả 201, xóa 204 không body theo contract đã chọn.

## Layer

Controller nhận HTTP; service xử lý quy tắc; repository đọc/ghi bộ nhớ. DTO mang dữ liệu ra/vào. **Tự làm:** vẽ một lần tạo task qua ba phần. **Gợi ý:** service nhận repository qua constructor.

## Validation

Title khác null/trắng, dài 1–100 sau trim; status hợp lệ; id không có trả 404. **Tự làm:** thử title có nhiều khoảng trắng hai đầu. **Gợi ý:** @Size không tự trim.

## Test

Kiểm service bằng Java không bật server; thêm HTTP test cho tạo, lỗi dữ liệu và không tìm thấy. **Tự làm:** cố tình làm sai validation. **Gợi ý:** test HTTP phải thấy status/body đúng, không chỉ method Java đúng.

## README Project

README ghi JDK/Boot đã chọn, cách chạy, gọi thử, giới hạn mất dữ liệu sau restart và cách chạy test. **Tự làm:** làm theo README trong terminal mới. **Gợi ý:** ghi cả port và dữ liệu đầu vào mẫu.

## Đọc thêm sau

Sang [API có database](../week-14/03-final-api-project.md) sau khi hiểu HTTP. Với nhiều request, không giữ current user/title trong field dùng chung; ConcurrentHashMap không tự bảo vệ nghiệp vụ nhiều bước.
