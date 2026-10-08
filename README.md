# Từ Frontend Đến Fullstack Java

Tài liệu dành cho lập trình viên frontend có khoảng 4 năm kinh nghiệm, mới bắt đầu Java và backend. Bạn đã biết lập trình; phần cần xây thêm là cách Java chạy, cách server xử lý yêu cầu và cách dữ liệu được bảo vệ trong database.

## Bắt đầu ở đâu?

1. Xem [chuẩn đầu ra](COURSE_OUTCOMES.md), rồi đọc [nhận xét tài liệu hiện tại](REVIEW.md) để hiểu những điểm cần đổi.
2. Theo [lộ trình 24 tuần](ROADMAP.md), dự kiến 8 giờ/tuần và điều chỉnh theo kết quả thực hành.
3. Làm [buổi đầu tiên: chạy và debug Java](learning-path/00-first-java-program.md).
4. Phát triển [Task Manager xuyên suốt](learning-path/03-task-manager-project.md), từ Java thuần đến ứng dụng fullstack.

## Cách dùng bộ bài cũ

Các thư mục `phase-*` và `week-*` giữ nguyên địa chỉ để bạn tra cứu. **Số tuần trong tên thư mục là mã của giáo trình cũ, không phải lịch mới.** Dùng bảng trong [ROADMAP.md](ROADMAP.md) để biết lúc nào đọc phần nào. Lịch 8h và checklist cũ là tài liệu tham khảo, không phải yêu cầu hoàn thành hết trong một tuần mới.

| Bạn đang cần | Đọc |
| --- | --- |
| Cài đặt, compile, chạy, debug | [Buổi đầu tiên](learning-path/00-first-java-program.md) |
| Hiểu request đi từ FE tới DB | [HTTP và trách nhiệm backend](learning-path/01-http-and-backend.md) |
| Session, ownership và luồng security | [Security căn bản](learning-path/05-security-basics.md) |
| JAR, config, container và CI | [Chạy ứng dụng](learning-path/06-running-and-ci.md) |
| Học database trước ORM | [SQL căn bản](learning-path/02-sql-before-jpa.md) |
| Biết phải làm và kiểm tra gì mỗi giai đoạn | [Project và tiêu chí nghiệm thu](learning-path/03-task-manager-project.md) |
| Java, collections, Spring và các bài chuyên sâu | [Bản đồ đọc bài](ROADMAP.md) |

## Cách học

Mỗi buổi: dự đoán kết quả → tự gõ ví dụ → chạy → sửa một đầu vào để tạo lỗi → giải thích lại bằng lời của bạn. Đọc 20–30 phút rồi viết code. Dùng AI để giải thích lỗi và review bài đã thử, tránh lấy nguyên lời giải trước khi làm.

Bản học này dùng Java 21 làm mốc cho ví dụ. Khi tạo Spring project, ghi rõ phiên bản vào README, dùng dependency management và Maven Wrapper của project. Không cần học đồng thời Java 17/21 hay nhiều major version Spring.
