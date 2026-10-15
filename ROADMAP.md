# Lộ Trình Frontend → Fullstack Java

## Đích đến và thời lượng

Dự kiến **24 tuần × 8 giờ**, có thể thêm thời gian củng cố. Đích đến là ứng dụng nhỏ có Java backend, FE quen thuộc, dữ liệu lưu bền vững và kiểm quyền. Đọc [chuẩn đầu ra](COURSE_OUTCOMES.md) khi cần tự đánh giá; không cần thuộc thuật ngữ ở các tuần chưa học.

## Một project xuyên suốt

Xây [Task Manager](learning-path/03-task-manager-project.md) từng bước: Java thuần → HTTP (cách client/server trao đổi) → SQL (ngôn ngữ dữ liệu) → Spring/JPA (công cụ dựng API và ánh xạ dữ liệu) → nối FE → kiểm quyền → chạy lại được.

## Lịch học và đầu ra

Tên week trong đường dẫn là mã bài cũ. Chỉ học phần được chọn; dùng README module để tìm bài cơ bản và bài tùy chọn.

### Tuần 1–4

| Tuần | Học và thực hành | Đầu ra |
| --- | --- | --- |
| 1 | [Chạy Java và đọc lỗi](learning-path/00-first-java-program.md) | Tự chạy Main và sửa lỗi dấu ;. |
| 2 | [Biến, class và dữ liệu](phase-01-java-language/week-01/01-type-system.md) | Tạo Task có title và chặn dữ liệu trống. |
| 3 | [Ghép object và kiểm thử](phase-01-java-language/week-01/02-oop-and-object-contract.md) | Thay sender; viết kiểm tra đầu tiên. |
| 4 | [Danh sách, kiểu phần tử và trạng thái](phase-02-collections-stream-io/week-04/00-collections-first.md) | Thêm/tìm/xóa task theo id. |

### Tuần 5–8

| Tuần | Học và thực hành | Đầu ra |
| --- | --- | --- |
| 5 | [Lỗi, record và lọc dữ liệu](phase-01-java-language/week-03/01-modern-java.md) | Lọc DONE bằng loop rồi Stream. |
| 6 | [FE trao đổi với server](learning-path/01-http-and-backend.md) | Viết dữ liệu/status của tạo và lấy task. |
| 7 | [Spring tạo và nối object](phase-06-spring-boot-core/week-13/01-ioc-di-bean-autoconfiguration.md) | Gọi được API nhỏ. |
| 8 | [API nhận dữ liệu và báo lỗi](phase-06-spring-boot-core/week-13/02-rest-validation-error-handling.md) | Thử 201/400/404; không giữ input trong field chung. |

### Tuần 9–12

| Tuần | Học và thực hành | Đầu ra |
| --- | --- | --- |
| 9 | [Bảng và đọc/ghi SQL](learning-path/02-sql-before-jpa.md) | Tạo bảng, thêm/đọc/sửa/xóa dòng. |
| 10 | [Ghép bảng và bảo vệ dữ liệu](learning-path/02-sql-before-jpa.md) | JOIN ghép bảng, thử COMMIT/ROLLBACK xác nhận/hủy. |
| 11 | [Java gọi database bằng JDBC](learning-path/02-sql-before-jpa.md) | SELECT với tham số tách khỏi câu SQL; đóng kết nối. |
| 12 | [Lưu object bằng JPA](phase-06-spring-boot-core/week-14/01-jpa-hibernate-transaction.md) | Restart API vẫn còn dữ liệu. |

### Tuần 13–16

| Tuần | Học và thực hành | Đầu ra |
| --- | --- | --- |
| 13 | [Thay cấu trúc DB có phiên bản](phase-06-spring-boot-core/week-14/02-flyway-integration-test-n-plus-one.md) | Dựng DB trống; lỗi giữa chừng không lưu dở. |
| 14 | [Quan hệ và chia trang kết quả](phase-06-spring-boot-core/week-14/03-final-api-project.md) | List task có thứ tự ổn định; đọc số truy vấn. |
| 15 | [Kiểm tra nhiều thành phần thật](phase-05-testing-design/week-11/README.md) | Test HTTP tới PostgreSQL riêng chạy thật. |
| 16 | [Nối giao diện hiện có](learning-path/03-task-manager-project.md) | Tạo/sửa từ FE, refresh không mất dữ liệu. |

### Tuần 17–20

| Tuần | Học và thực hành | Đầu ra |
| --- | --- | --- |
| 17 | [Đăng nhập và phiên người dùng](learning-path/05-security-basics.md) | Login/logout; chặn người chưa đăng nhập. |
| 18 | [Kiểm quyền từng dữ liệu](learning-path/05-security-basics.md) | B không thao tác được task của A qua API. |
| 19 | [Cấu hình và tìm lỗi khi chạy](learning-path/06-running-and-ci.md) | Tắt DB bài tập, lần theo lỗi rồi khôi phục. |
| 20 | [Đóng gói để chạy lại](learning-path/06-running-and-ci.md) | Chạy JAR rồi container; restart vẫn giữ DB. |

### Tuần 21–24

| Tuần | Học và thực hành | Đầu ra |
| --- | --- | --- |
| 21 | [Tự động kiểm tra bằng CI](learning-path/06-running-and-ci.md) | Test cố ý fail làm quy trình fail; người khác chạy lại được. |
| 22 | [Hai yêu cầu sửa cùng dữ liệu](phase-03-concurrency/week-06/01-thread-and-race-condition.md) | Phát hiện cập nhật từ version cũ thay vì ghi đè. |
| 23 | [JVM vừa đủ và cải thiện code](phase-04-jvm-performance/week-09/01-jvm-memory-classloading.md) | Vẽ object/lời gọi; refactor một chỗ có test. |
| 24 | [Tự thêm tính năng xuyên ứng dụng](COURSE_OUTCOMES.md) | Thêm dueDate và demo từ môi trường mới. |

Tuần 4–5 khá nhiều ý: học [Generics](phase-01-java-language/week-02/01-generics.md), [enum](phase-01-java-language/week-02/02-enum-and-nested-class.md), [exception](phase-01-java-language/week-03/02-exception-handling.md), [Stream](phase-02-collections-stream-io/week-05/01-stream-optional-functional.md) từng lượt; chưa xong thì thêm buổi, không cộng thời lượng mọi module vào một tuần.

## Mốc dừng để củng cố

- Sau tuần 5: tự viết và kiểm nghiệp vụ Java nhỏ.
- Sau tuần 8: giải thích được request đi qua API và lỗi trả về.
- Sau tuần 15: DB mới dựng được, dữ liệu còn sau restart, thao tác lỗi không lưu dở.
- Sau tuần 18: hai tài khoản chứng minh kiểm quyền.
- Sau tuần 24: thêm tính năng nhỏ và chạy lại theo README.

## Những phần học sau khi đã có ứng dụng

Các mục tự chọn được đưa xuống “Đọc thêm sau” ở cuối file để không chen giữa đường học chính. Không phải điều kiện để bắt đầu Spring.

## Nhịp 8 giờ mỗi tuần

Bốn buổi khoảng 2 giờ: hiểu ví dụ → tự làm → kiểm trường hợp lỗi → sửa và ghi chú. Trong một buổi, chia lượt đọc 15–25 phút rồi gõ code. Nếu cài công cụ tốn thời gian, giảm phạm vi bài; đừng bỏ kiểm tra.

## Nguồn tra cứu theo phiên bản

Ví dụ dùng Java 21. Dùng [Java 21 API](https://docs.oracle.com/en/java/javase/21/docs/api/) để tra method; khi tạo Spring project, kiểm [yêu cầu hệ thống](https://docs.spring.io/spring-boot/system-requirements.html), giữ phiên bản do project chọn và ghi lại. Không trộn cấu hình từ nhiều major version (nhánh phiên bản lớn).

## Đọc thêm sau

- [Generics/nested class nâng cao](phase-01-java-language/week-02/06-advanced-reference.md).
- [Sealed class và try-with-resources](phase-01-java-language/week-03/06-advanced-reference.md).
- [Concurrency chuyên sâu](phase-03-concurrency/README.md): nhiều công việc cùng tiến hành.
- [JVM chuyên sâu](phase-04-jvm-performance/README.md): máy thực thi Java và chẩn đoán.
- Tự viết cache/parser/thread pool khi có mục tiêu học riêng; hệ thống phân tán và vận hành quy mô lớn ngoài đường bắt buộc.
