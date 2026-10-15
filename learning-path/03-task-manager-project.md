# Project Xuyên Suốt: Task Manager

Theo lịch 24 tuần trong [ROADMAP](../ROADMAP.md). Chia mỗi chặng thành bước nhỏ; giữ lịch 8 giờ/tuần cho tổng công việc.

## Phạm vi và quy tắc

Task Manager là ứng dụng quản lý công việc được mở rộng từng chặng. Mỗi task có title sau trim dài 1–100, status TODO/IN_PROGRESS/DONE; bước đầu cho đổi qua lại ba trạng thái. Làm một ứng dụng xuyên suốt, không bắt đầu lại mỗi module.

## Chặng A — Java thuần, tuần 1–5

Chặng A: xử lý quy tắc bằng Java thuần.

- Tạo Task, trạng thái và thao tác đổi title.
- Lưu tạm trong Map theo id.
- Kiểm đầu vào đúng, sai và id không có.

**Gợi ý:** Làm một method mỗi buổi; title không phụ thuộc giao diện.

**Đạt khi:** Chạy từ main hoặc JShell và giải thích từng lỗi. Tự gõ phần triển khai, không dùng AI sinh lời giải.

## Chặng B — HTTP API, tuần 6–8

Chặng B: HTTP API là điểm FE gọi vào bằng request/response.

- Chốt POST/GET/PUT/DELETE cho task.
- Dùng controller nhận HTTP, service xử lý, repository lưu tạm.
- Kiểm 201/400/404 và 204 không body khi xóa thành công.

**Gợi ý:** Theo bài HTTP rồi Spring; chưa thêm database.

**Đạt khi:** Gọi bằng curl và chỉ được request đi qua những class nào. Tự gõ phần triển khai, không dùng AI sinh lời giải.

## Chặng C — PostgreSQL, tuần 9–15

Chặng C: PostgreSQL lưu dữ liệu còn sau khi tắt chương trình.

- Viết bảng và SQL cơ bản trước JPA.
- Dùng migration để dựng lại cấu trúc DB.
- Kiểm transaction rollback và đọc query cho task/project.

**Gợi ý:** Migration là file thay đổi schema có phiên bản; rollback hủy thay đổi chưa commit.

**Đạt khi:** DB mới dựng được và dữ liệu còn sau restart; test không dùng DB demo. Tự gõ phần triển khai, không dùng AI sinh lời giải.

## Chặng D — Nối frontend, tuần 16

Chặng D: nối frontend với API đã có.

- Tạo/sửa task từ form.
- Hiển thị loading, rỗng và lỗi.
- Refresh và kiểm dữ liệu vẫn đúng.

**Gợi ý:** Dùng tab Network để so request với contract.

**Đạt khi:** FE hiển thị lỗi validation từ server đúng field. Tự gõ phần triển khai, không dùng AI sinh lời giải.

## Chặng E — Đăng nhập và quyền, tuần 17–18

Chặng E: xác thực và kiểm quyền dữ liệu.

- Dùng session để xác định người gọi.
- Kiểm ownership (quyền sở hữu) ở server.
- Thử tài khoản B với task của A.

**Gợi ý:** Không lấy ownerId từ body như danh tính đáng tin; xem bài security.

**Đạt khi:** B không đọc/sửa/xóa được task của A, kể cả gọi API trực tiếp. Tự gõ phần triển khai, không dùng AI sinh lời giải.

## Chặng F — Chạy lại được và xử lý lỗi, tuần 19–21

Chặng F: người khác chạy lại được ứng dụng.

- Chạy executable JAR ngoài IDE.
- Cấu hình DB bằng biến môi trường.
- Tạo quy trình CI chạy test thật và build.

**Gợi ý:** Đóng gói container sau khi chạy JAR được; dữ liệu ở nơi lưu bền vững.

**Đạt khi:** README đủ bước chạy và thử một test fail làm CI fail. Tự gõ phần triển khai, không dùng AI sinh lời giải.

## Chặng G — Cập nhật đồng thời và tổng kết, tuần 22–24

Chặng G: tránh cập nhật ghi đè nhau ngoài ý muốn.

- Hai người cùng đọc một task.
- Cho cả hai cập nhật từ cùng version.
- Từ chối một lần cập nhật cũ theo contract.

**Gợi ý:** Version là số phiên bản dữ liệu; kiểm nó ở DB cùng thao tác ghi.

**Đạt khi:** Không âm thầm mất thay đổi; FE xử lý conflict (xung đột) rõ. Tự gõ phần triển khai, không dùng AI sinh lời giải.

## Nghiệm thu cuối

Nghiệm thu bằng việc thêm dueDate (ngày hết hạn) xuyên FE → API → DB, kèm kiểm tra đầu vào, quyền và lưu trữ. Làm theo [chuẩn đầu ra](../COURSE_OUTCOMES.md); chỉ đọc hết tài liệu chưa được tính là hoàn thành.

## Đọc thêm sau

Tự viết parser, cache, thread pool, tối ưu GC và hệ thống phân tán là nhánh tự chọn sau ứng dụng. Chỉ chọn khi giải quyết được một vấn đề cụ thể.
