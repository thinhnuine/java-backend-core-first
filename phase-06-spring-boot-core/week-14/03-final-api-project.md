# Final Project: Task API Hoặc Notes API

Đây là đề để tự làm, không phải đáp án. Chọn từng mục theo README module; không cần làm hết trong một buổi. Tự gõ code, dùng AI để xin gợi ý sau khi đã thử.

## Mục Tiêu

Đây là mốc API lưu PostgreSQL trong tuần 12–15 mới, chưa phải nghiệm thu cuối khóa. Tiếp tục trên Task Manager đang có.

## Yêu Cầu Tối Thiểu

CRUD (tạo/đọc/sửa/xóa), PostgreSQL, migration (thay cấu trúc DB có phiên bản), validation và lỗi nhất quán. **Tự làm:** hoàn thiện tạo/lấy trước. **Gợi ý:** kiểm dữ liệu từ DB sau restart.

## Domain Gợi Ý: Task API

Project chứa id/name/createdAt. Task thêm projectId, title, description, status, dueDate, createdAt/updatedAt. **Tự làm:** vẽ bảng và khóa liên kết trước entity. **Gợi ý:** dueDate là LocalDate, thời điểm tạo là Instant.

## Endpoint Gợi Ý

Tạo/lấy project; tạo/list task trong project; đọc/sửa/xóa task theo id. **Tự làm:** xác định contract khi project không tồn tại. **Gợi ý:** danh sách cần thứ tự ổn định trước khi phân trang.

## README Cần Có

Ghi cách tạo DB bài tập, chạy migration, cấu hình, chạy app/test và gọi API mẫu. **Tự làm:** dựng từ DB trống. **Gợi ý:** giữ credential thật ngoài Git, file mẫu chỉ ghi tên biến.

## Acceptance

- [ ] Dữ liệu còn sau restart.
- [ ] Migration chạy trên DB trống.
- [ ] Một lỗi giữa hai bước ghi làm rollback đúng.
- [ ] Có test HTTP đến DB thật và người khác chạy lại được.

**Tự làm:** trình bày một query chính. **Gợi ý:** giải thích SQL, không chỉ tên method repository.

## Đọc thêm sau

N+1 và phân trang với quan hệ học khi xem query thực tế. Sau mốc DB, làm [nối FE, kiểm quyền và chạy lại ứng dụng](../../learning-path/03-task-manager-project.md), rồi [nghiệm thu cuối](../../COURSE_OUTCOMES.md).
