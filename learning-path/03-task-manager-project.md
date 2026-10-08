# Project Xuyên Suốt: Task Manager

Đây là đề thực hành cho [lộ trình 24 tuần](../ROADMAP.md), không phải lời giải có sẵn. Mỗi tuần dành khoảng 4–5 giờ triển khai và test. Dùng frontend stack bạn đang quen; mục tiêu là dành phần lớn công sức cho Java và backend.

## Phạm vi và quy tắc

Task có `id`, `title`, `status`, `createdAt`; thêm `projectId`, `ownerId`, `version` ở đúng chặng. Title sau khi trim dài 1–100 ký tự; status là `TODO`, `IN_PROGRESS`, `DONE`. ID do server tạo, createdAt do server gán. Chưa làm kéo thả, upload, email, realtime hoặc đa vai trò quản trị.

Giai đoạn đầu cho phép chuyển qua lại giữa ba status để tránh nghiệp vụ phức tạp. Tới phần quyền, mỗi project thuộc một user, task thuộc project và cùng chủ sở hữu; người dùng chỉ thao tác dữ liệu của mình. Dữ liệu mẫu trước khi có auth cần migration gán owner rõ ràng.

## Chặng A — Java thuần, tuần 1–5

Tạo Task, TaskStatus, TaskService và repository in-memory sau khi đã học collection. Ban đầu gọi service từ `main`; không cần giao diện menu nhập liệu. Interface repository chỉ xuất hiện khi bạn có nhu cầu thay in-memory bằng DB hoặc fake trong test.

Thứ tự: tạo task hợp lệ → từ chối title sai → tìm theo ID → đổi status → liệt kê theo status. Tuần 3 dùng [bài unit test đầu tiên](04-first-unit-test.md) để bắt đầu test. Chưa cần Mockito; fake repository đơn giản là đủ.

Nghiệm thu:

- [ ] Title có khoảng trắng được chuẩn hóa; null/blank/quá dài bị từ chối.
- [ ] Hai task được cấp ID khác nhau; tìm ID không tồn tại có kết quả rõ.
- [ ] Đổi status không làm đổi task khác.
- [ ] Caller không thể tự sửa collection nội bộ ngoài API dự định.
- [ ] Tự giải thích dependency được đưa vào service qua constructor.

## Chặng B — HTTP API, tuần 6–8

Tạo Spring project với Maven, Java 21, dependency cho Spring Web và Validation; dùng phiên bản stable được công cụ tạo project hỗ trợ. Giữ Maven Wrapper và phiên bản cụ thể trong Git. Chuyển nghiệp vụ đã có vào service và thêm controller/DTO theo [bài API](../phase-06-spring-boot-core/week-13/03-in-memory-task-api-project.md).

| Endpoint | Mục đích | Case phải thử |
| --- | --- | --- |
| POST /tasks | Tạo task | 201; title blank → 400 |
| GET /tasks/{id} | Đọc một task | 200; không có → 404 |
| GET /tasks?status=DONE | Lọc task | 200; status sai → 400 |
| PATCH /tasks/{id} | Đổi title/status theo contract | 200; dữ liệu sai → 400; không có → 404 |
| DELETE /tasks/{id} | Xóa task | 204; không có → 404 theo quy ước bài |

PATCH ở bài này chỉ nhận title/status; trường vắng mặt giữ nguyên, explicit null bị từ chối. Phải phân biệt hai trường hợp khi deserialize, hoặc chọn endpoint cập nhật status chuyên biệt nếu chưa muốn xử lý partial update tổng quát. Ghi lựa chọn vào contract. Các bài cũ dùng PUT; nếu chọn PUT hãy định nghĩa body thay thế đầy đủ, đừng gọi mọi cập nhật một field là PUT mà không giải thích.

Trước khi nhận request đồng thời, rà repository in-memory: ID generation và cập nhật nhiều bước cần được bảo vệ. Có thể đồng bộ hóa thao tác repository của demo bằng một lock; map thread-safe đơn lẻ không bảo vệ cả chuỗi check–update. Không để request hiện tại hay user hiện tại trong field singleton service.

Nghiệm thu: gọi được bằng curl, error schema nhất quán, test lỗi validation/not found và giải thích restart làm mất dữ liệu vì repository hiện còn ở RAM.

## Chặng C — PostgreSQL, tuần 9–15

Làm [SQL trước JPA](02-sql-before-jpa.md), rồi thay repository bằng JPA. Tới đây thêm Project và quan hệ task thuộc project. Tạo task qua `POST /projects/{projectId}/tasks`; sửa contract và test tương ứng, không giữ hai cách tạo mâu thuẫn nhau.

Thêm Flyway migration, cấu hình Hibernate kiểm tra schema thay vì tự sửa schema, test bằng PostgreSQL riêng hoặc Testcontainers. Không sửa migration đã áp dụng ở môi trường chia sẻ; thêm migration mới. Log SQL chỉ bật có chủ đích cho local/test.

Phân trang chọn page bắt đầu từ 0, size mặc định 20, tối đa 100, sort theo `createdAt DESC, id DESC`. Response có `items`, `page`, `size`, `totalElements`; input ngoài miền trả 400. Đừng phụ thuộc trực tiếp vào định dạng serialize nội bộ của framework cho public contract.

Bài transaction cụ thể: tạo project cùng hai task trong một service method. Cố tình gây lỗi khi ghi task thứ hai; kiểm tra project và task thứ nhất đều không được commit. Chạy qua Spring bean thật để test transaction, không chỉ gọi object tự `new`.

Nghiệm thu:

- [ ] DB trống được dựng bằng migration; API vẫn có dữ liệu sau restart.
- [ ] Constraint chặn dữ liệu sai; DTO không lộ entity internals.
- [ ] Integration test kiểm CRUD, rollback và phân trang trên PostgreSQL.
- [ ] Xem query list task/project, giải thích có N+1 hay không từ SQL đã quan sát.
- [ ] Test chạy cô lập, không dùng DB đang demo; cấu hình test được chạy thật trong `./mvnw verify` và kiểm báo cáo số test, không chỉ thấy BUILD SUCCESS.

## Chặng D — Nối frontend, tuần 16

Làm ba màn hình đơn giản: danh sách task, tạo task và chỉnh sửa. Loading, empty, validation error, network error đều có trạng thái rõ. Sau create/update thành công, cập nhật hoặc tải lại dữ liệu theo API contract. Không ưu tiên optimistic UI khi còn đang kiểm tra tính đúng của server.

Nghiệm thu: tạo trên browser, reload vẫn còn; gọi lỗi bằng API client và thấy FE hiển thị đúng; phân biệt lỗi mạng, 400 và 500 bằng Network tab. FE không chứa DB credential.

## Chặng E — Đăng nhập và quyền, tuần 17–18

Bắt đầu với [bài session/kiểm quyền](05-security-basics.md); xác định login/CSRF/error contract trước khi ghép frontend.

Authentication trả lời “bạn là ai”; authorization trả lời “bạn được làm gì”. Bắt đầu bằng Spring Security với username/password và server-side session. Học cookie session trước; JWT/OAuth là nhánh học tiếp khi có nhu cầu cụ thể.

Đọc [username/password authentication](https://docs.spring.io/spring-security/reference/servlet/authentication/passwords/index.html). Dùng cơ chế [PasswordEncoder của Spring Security](https://docs.spring.io/spring-security/reference/features/authentication/password-storage.html), không lưu plaintext hoặc tự thiết kế hash password. Ban đầu tạo hai tài khoản mẫu ở môi trường dev; chưa cần luồng đăng ký/reset password.

Với session cookie, giữ và hiểu [CSRF protection](https://docs.spring.io/spring-security/reference/servlet/exploits/csrf.html). FE cần gửi CSRF token cho yêu cầu thay đổi dữ liệu theo cấu hình; sau login/logout xử lý việc lấy token mới nếu cần. Không tắt CSRF chỉ để hết lỗi 403. Cookie session cần HttpOnly và Secure khi chạy HTTPS; chọn SameSite theo cách deploy.

Ưu tiên cùng origin qua proxy khi deploy demo. Nếu FE/API khác origin, cấu hình origin cụ thể, credentials và preflight; FE gửi cookie với chế độ credentials phù hợp. CORS không thay thế kiểm quyền.

Checklist dùng hai tài khoản A/B:

- [ ] Chưa đăng nhập gọi endpoint riêng tư nhận 401 theo API contract.
- [ ] A tạo project/task; B không đọc, sửa, xóa được task/project của A bằng cách đoán ID.
- [ ] Danh sách và số lượng phân trang chỉ tính dữ liệu của user hiện tại.
- [ ] Thử gửi ownerId của A khi đang là B; server lấy identity từ security context, không tin ownerId trong body.
- [ ] POST/PATCH/DELETE thiếu hoặc sai CSRF token bị từ chối khi dùng session cookie.
- [ ] Logout làm session cũ không còn truy cập được; không chỉ xóa state FE.

## Chặng F — Chạy lại được và xử lý lỗi, tuần 19–21

Đọc [bài chạy ứng dụng và CI](06-running-and-ci.md) để hiểu JAR, env, container network/volume và cách chắc chắn test đã chạy.

Học theo thứ tự: đóng gói JAR → chạy bằng env config → container API → kết nối DB → ghép FE/API/DB → CI → demo. Container là cách đóng gói process và môi trường chạy; volume giữ dữ liệu DB qua việc thay container. Không tự động coi stop/restart là xóa database.

README của project phải có phiên bản JDK/framework/DB, lệnh setup, biến môi trường mẫu không chứa secret, lệnh migration/test/run, URL kiểm tra và cách dừng. Pin phiên bản image dùng cho demo, không dùng `latest` như một cấu hình tái lập.

Bài thực hành vận hành:

1. Tắt DB trong môi trường bài tập; quan sát lỗi phía FE, response và log API.
2. Cấu hình connection timeout; kiểm tra request không treo vô hạn. Không thử lại mọi lỗi một cách mù quáng.
3. Có health/readiness để phân biệt process còn sống với khả năng phục vụ DB; không công khai config/secret từ endpoint quản trị.
4. CI chạy `./mvnw verify` và build frontend; fail khi test fail. Giữ dữ liệu và credential test tách biệt.
5. Dựng demo HTTPS nếu có hosting phù hợp; hoặc nghiệm thu bằng môi trường local tái lập. Chi phí hosting không phải điều kiện để hoàn thành bài.

## Chặng G — Cập nhật đồng thời và tổng kết, tuần 22–24

Thêm optimistic locking với version. Hai client đọc cùng task/version, gửi hai thay đổi khác nhau: request dùng version cũ phải bị từ chối bằng 409 theo contract. FE cho tải lại dữ liệu để người dùng quyết định. Chỉ thêm `@Version` chưa đủ chứng minh chống mọi stale edit: request phải mang version kỳ vọng và server kiểm tra nó cùng cơ chế khóa của JPA.

Refactor một service dài thành các phần dễ hiểu sau khi có test. Học heap/stack ở mức biết dữ liệu sống ở đâu và đọc lỗi; chưa cần tuning GC để hoàn thành Task Manager.

## Nghiệm thu cuối

- [ ] Người khác chạy được từ README với DB sạch và biến môi trường mẫu.
- [ ] Migration, unit test và integration test chạy thành công.
- [ ] Demo từ FE: login → tạo project/task → sửa → reload → logout.
- [ ] Demo bằng API: input sai, không tồn tại, truy cập chéo user, conflict version.
- [ ] Tự thêm field `dueDate` qua migration, entity, DTO, API, form và test; giải thích vì sao ngày hạn không nhất thiết là một timestamp UTC.
- [ ] Mô tả một lỗi từng gặp, bằng chứng tìm được và cách sửa bằng lời của mình.

Tiếp theo: chọn một bài [concurrency](../phase-03-concurrency/README.md) hoặc [JVM](../phase-04-jvm-performance/README.md) từ vấn đề bạn thật sự gặp khi làm project.

Chuẩn nghiệm thu toàn khóa và bài sửa độc lập nằm tại [COURSE_OUTCOMES.md](../COURSE_OUTCOMES.md).
