# Chuẩn Đầu Ra: Từ FE Đến Fullstack Java

## Mục tiêu của khóa

Sau khi hoàn thành phần bắt buộc và bài nghiệm thu, bạn có thể tự triển khai, kiểm thử và giải thích một tính năng nhỏ xuyên frontend → Java API → PostgreSQL. Bạn sử dụng kinh nghiệm FE đang có, đồng thời xây nền Java và trách nhiệm phía server để nhận công việc fullstack có người review.

“Hoàn thành” được đánh giá bằng code và hành vi quan sát được, không bằng việc đọc hết các file. Khóa cung cấp nền làm ứng dụng nhỏ và đọc code backend phổ biến; kinh nghiệm xử lý hệ thống lớn, tuning JVM, distributed systems và vận hành production cần tiếp tục tích lũy trong công việc.

## Sáu năng lực và bằng chứng

| Năng lực | Bạn cần tự làm được | Bằng chứng nghiệm thu |
| --- | --- | --- |
| Java căn bản | Class, constructor, interface/composition, collection, null/equality, exception; đọc stack trace | TaskService Java thuần có unit test hợp lệ, lỗi và case biên |
| HTTP/Spring | Contract, DTO, controller/service/repository, DI, validation và error | Gọi API bằng curl; giải thích 201/400/404; chỉ ra request đi qua đâu |
| Dữ liệu | Schema/key/constraint/JOIN, migration, JDBC căn bản, JPA và transaction | DB sạch dựng bằng migration; restart còn dữ liệu; test rollback; đọc SQL của truy vấn chính |
| Tích hợp FE | Form, loading/empty/error, refresh dữ liệu, phân trang ổn định | Tạo/sửa task từ browser; reload vẫn đúng; hiển thị lỗi từ API |
| Kiểm quyền | Login/logout bằng session; nguồn identity đáng tin; ownership; CORS/CSRF cơ bản | Hai tài khoản A/B; B bị chặn khi truy cập task của A bằng API trực tiếp |
| Chất lượng và chạy lại | Unit/integration test, env config, log, package, container/CI căn bản | verify chạy test thật; người khác chạy theo README; lần một lỗi từ FE đến log/DB |

## Bài nghiệm thu cuối

Dùng Task Manager đã xây; thêm dueDate kiểu ngày và lọc task theo status/dueDate. Tự xác định request/response trước khi code, thêm migration, cập nhật Java/DTO/FE và test. Không bắt đầu một ứng dụng mới để nghiệm thu.

Thực hiện bốn phần:

1. **Build và demo:** từ DB bài tập mới, dựng schema, chạy test và app theo README. Login → tạo project/task → sửa dueDate → lọc → reload → logout.
2. **Case thất bại:** title không hợp lệ, JSON sai, ID không tồn tại, truy cập chéo user, thiếu CSRF ở yêu cầu dùng session cookie, cập nhật với version cũ.
3. **Giải thích:** vẽ luồng request, chỉ transaction boundary, giải thích constraint và query chính. Đặt breakpoint, đọc một stack trace, chỉ ra điều gì xảy ra khi DB không kết nối được.
4. **Sửa độc lập:** nhận thêm một yêu cầu nhỏ hoặc test fail, tự sửa từng bước rồi giải thích tradeoff. Có thể tra tài liệu và hỏi gợi ý; cần hiểu và chịu trách nhiệm từng phần code mình nộp.

Không yêu cầu số test hay coverage phần trăm tùy ý. Test phải bắt được rule quan trọng và từng lỗi đã tìm thấy.

## Cách quyết định đạt

Một năng lực đạt khi bạn vừa thực hiện được, vừa giải thích được, vừa chứng minh case lỗi liên quan. Nếu chỉ demo happy path thì chưa đạt. Nếu còn truy cập chéo user, mất dữ liệu sau restart ngoài dự định, migration không chạy từ DB trống hoặc không có test DB thật cho flow chính, tiếp tục củng cố trước khi nghiệm thu.

Reviewer ghi cho mỗi năng lực: Đạt / Cần củng cố, kèm một bằng chứng cụ thể. Không dùng tổng điểm để bù một lỗi kiểm quyền bằng việc giao diện đẹp.

## Thời lượng thực tế và hướng tiếp theo

Lịch 24 tuần × 8 giờ là mốc lập kế hoạch ban đầu. Tuần 4–5, SQL/JPA, auth và Docker có nhiều công cụ mới; có thể cần thêm 4–8 tuần củng cố tùy tốc độ thực hành. Đi tiếp khi qua mốc, giảm bài tự chọn nếu quá tải.

Sau khóa, ưu tiên một vài tính năng thực tế trong team có review: thêm endpoint nhỏ, migration, validation, integration test và xử lý lỗi. Học concurrency/JVM chuyên sâu theo vấn đề gặp được. Khóa không quyết định chức danh hay bảo đảm chuyển việc; chuẩn trên cho biết năng lực nào đã có bằng chứng và phần nào cần luyện thêm.

Tiếp theo: [roadmap](ROADMAP.md), [đề Task Manager](learning-path/03-task-manager-project.md), [bản review toàn bộ](REVIEW.md).
