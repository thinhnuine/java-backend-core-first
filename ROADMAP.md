# Lộ Trình Frontend → Fullstack Java

## Đích đến và thời lượng

Chuẩn đánh giá chi tiết nằm trong [COURSE_OUTCOMES.md](COURSE_OUTCOMES.md). Sau lộ trình, bạn có thể tự xây một ứng dụng nhỏ: giao diện quen thuộc gọi Java API, dữ liệu ở PostgreSQL, có đăng nhập/phân quyền, test và quy trình chạy lại từ máy sạch. Đây là nền tảng để nhận việc fullstack có review, không phải cam kết đạt cấp độ backend senior trong vài tháng.

Theo thời gian bạn xác nhận, mốc dự kiến: **24 tuần × 8 giờ ≈ 192 giờ**, cộng thời gian bù khi chưa qua tiêu chí. Nên dự phòng thêm 4–8 tuần củng cố, đặc biệt ở SQL/JPA, auth và công cụ chạy ứng dụng; đây là ước lượng giảng dạy, không phải thời hạn cố định. Khoảng 4 giờ/tuần có thể cần khoảng 48 tuần; 12 giờ/tuần có thể khoảng 16 tuần. Đây là quy đổi khối lượng để lập kế hoạch, không phải hạn phải chạy theo.

Không giả định bạn đã biết SQL, Node.js backend, Docker hay authentication. Kiến thức FE giúp ở UI và API client; phần server vẫn học từ gốc. Java 21 là mốc ví dụ; framework học một phiên bản ổn định phù hợp, không dùng preview cho bài bắt buộc.

## Một project xuyên suốt

[Task Manager](learning-path/03-task-manager-project.md): quản lý task cá nhân, sau đó nhóm theo project và giới hạn dữ liệu theo người đăng nhập. Ban đầu dữ liệu mất khi tắt chương trình là bình thường; mỗi chặng giải quyết thêm một vấn đề cụ thể.

```text
Java chạy local → nghiệp vụ + test → HTTP API in-memory
→ SQL + JDBC → API dùng PostgreSQL/JPA → FE tích hợp
→ đăng nhập + phân quyền → kiểm thử + vận hành demo
```

## Lịch học và đầu ra

Các đường dẫn `week-*` bên dưới là **mã bài cũ**. Chỉ đọc phần được chỉ định, không cần hoàn thành toàn bộ bài tập của module cũ.

| Tuần mới | Nội dung phải hiểu | Đọc và thực hành | Bằng chứng hoàn thành |
| --- | --- | --- | --- |
| 1 | JDK/JVM, compile/run, main, method, if/loop, debug | [Buổi đầu](learning-path/00-first-java-program.md) | Chạy bằng terminal và IDE; đặt breakpoint; đọc được một stack trace |
| 2 | Primitive/reference, null, String, class, constructor, access | [Type system](phase-01-java-language/week-01/01-type-system.md): đọc phần package trước, rồi primitive/String/boxing | Class Task có title; chặn null/blank; giải thích `==` và `equals` |
| 3 | Composition, interface, final; test cơ bản | [OOP](phase-01-java-language/week-01/02-oop-and-object-contract.md): interface/composition/immutability; [Unit test đầu tiên](learning-path/04-first-unit-test.md) | Service Java thuần có test tạo task hợp lệ và từ chối title rỗng |
| 4 | List/Set/Map, equals/hashCode, generic cơ bản, enum | [Collections căn bản](phase-02-collections-stream-io/week-04/00-collections-first.md), [generics cơ bản](phase-01-java-language/week-02/01-generics.md), [enum](phase-01-java-language/week-02/02-enum-and-nested-class.md) | In-memory repository thêm/tìm/xóa task; test ID không tồn tại |
| 5 | Exception, record, Optional, Stream vừa đủ, thời gian | [Exceptions](phase-01-java-language/week-03/02-exception-handling.md), [record](phase-01-java-language/week-03/01-modern-java.md), [Stream](phase-02-collections-stream-io/week-05/01-stream-optional-functional.md), [time](phase-02-collections-stream-io/week-05/02-java-time-and-nio.md) | Lọc task DONE bằng loop và Stream; giải thích lỗi nghiệp vụ và lỗi lập trình |
| 6 | HTTP, JSON, status, validation, client/server | [HTTP nền tảng](learning-path/01-http-and-backend.md) | Viết contract tạo/lấy task với response thành công và thất bại |
| 7 | Spring Boot, DI, controller → service → repository | [IoC/DI](phase-06-spring-boot-core/week-13/01-ioc-di-bean-autoconfiguration.md) | API tạo/lấy task chạy local; giải thích được object do ai tạo |
| 8 | REST, DTO, validation, error, shared state | [REST](phase-06-spring-boot-core/week-13/02-rest-validation-error-handling.md), [project in-memory](phase-06-spring-boot-core/week-13/03-in-memory-task-api-project.md), [race condition](phase-03-concurrency/week-06/01-thread-and-race-condition.md) | Test 201/400/404; không giữ request/user hiện tại trong field service |
| 9 | Table, row, primary/foreign key, CRUD SQL | [SQL nền tảng](learning-path/02-sql-before-jpa.md) mục schema và truy vấn | Tự tạo bảng và viết INSERT/SELECT/UPDATE/DELETE |
| 10 | JOIN, GROUP BY, constraint, transaction, index | [SQL nền tảng](learning-path/02-sql-before-jpa.md) phần còn lại | JOIN task/project; demo ROLLBACK; giải thích constraint |
| 11 | JDBC, parameter binding, connection/resource | Bài JDBC trong [SQL nền tảng](learning-path/02-sql-before-jpa.md) | Java thực thi một SELECT tham số hóa; đóng tài nguyên |
| 12 | JPA entity/repository, DTO mapping | [JPA](phase-06-spring-boot-core/week-14/01-jpa-hibernate-transaction.md) | API đọc/ghi PostgreSQL; restart app vẫn còn dữ liệu |
| 13 | Migration, transaction boundary, rollback | [Flyway](phase-06-spring-boot-core/week-14/02-flyway-integration-test-n-plus-one.md) | DB trống được dựng từ migration; test thao tác nhiều ghi cùng rollback |
| 14 | Quan hệ, lazy loading, N+1, pagination | [Bài DB API](phase-06-spring-boot-core/week-14/03-final-api-project.md) | Phân trang có thứ tự ổn định; xem SQL để giải thích query |
| 15 | Unit/integration test, PostgreSQL test riêng | [Testing](phase-05-testing-design/week-11/README.md) và checklist project | Test API đi qua DB thật, dữ liệu test cô lập; `mvn verify` chạy test |
| 16 | Nối frontend, form error, loading/empty/error | [Project chặng D](learning-path/03-task-manager-project.md) | Tạo/sửa task từ FE; refresh vẫn thấy dữ liệu |
| 17 | Authentication, password hashing, session | [Session và kiểm quyền](learning-path/05-security-basics.md) | Login/logout; endpoint riêng tư từ chối người chưa đăng nhập |
| 18 | Authorization, ownership, CORS, CSRF | [Session và kiểm quyền](learning-path/05-security-basics.md) | User B không đọc/sửa/xóa task của A; test trực tiếp API |
| 19 | Connection timeout, config, log, health check | [Chạy ứng dụng và CI](learning-path/06-running-and-ci.md) | Tắt DB để quan sát lỗi; trace lỗi từ response đến log |
| 20 | Maven package, Docker cơ bản, env, persistence | [Chạy ứng dụng và CI](learning-path/06-running-and-ci.md) | Chạy FE/API/DB theo README; dữ liệu còn sau restart bình thường |
| 21 | CI và môi trường demo | [Chạy ứng dụng và CI](learning-path/06-running-and-ci.md) | Pipeline chạy test/build; có URL demo hoặc môi trường local tái lập |
| 22 | Concurrent requests, lost update, optimistic locking | [Race condition](phase-03-concurrency/week-06/01-thread-and-race-condition.md) và thử nghiệm project | Hai cập nhật cùng version có một bên nhận conflict; không ghi đè âm thầm |
| 23 | JVM vừa đủ, debug, refactor | [JVM memory](phase-04-jvm-performance/week-09/01-jvm-memory-classloading.md), [SOLID](phase-05-testing-design/week-12/01-solid-practical.md) | Giải thích heap/stack; refactor một chỗ có test bảo vệ |
| 24 | Tổng kết và demo độc lập | Checklist nghiệm thu cuối trong [project](learning-path/03-task-manager-project.md) | Tự thêm một field xuyên FE/API/DB và test; demo từ máy/môi trường sạch |

Tuần 4–5 gồm nhiều bài nền: chỉ làm ví dụ nhỏ được chọn. Nếu cần đủ 8 giờ cho record/exception hoặc collections, chuyển phần còn lại sang buổi/tuần củng cố trước HTTP; không cộng lịch 8h của từng module lại thành một tuần.

## Mốc dừng để củng cố

- Cuối tuần 5: viết và test nghiệp vụ Java thuần. Nếu còn phải đoán constructor/interface/collection làm gì, dành thêm vài buổi ở đây.
- Cuối tuần 8: dùng curl gọi API và giải thích từng bước xử lý. Đừng học JPA để bù cho việc chưa hiểu HTTP.
- Cuối tuần 15: tự viết SQL tương đương với truy vấn chính; test rollback và migration trên PostgreSQL.
- Cuối tuần 18: chứng minh server kiểm quyền bằng hai tài khoản; ẩn nút ở FE không được tính là phân quyền.
- Cuối tuần 24: người khác làm theo README chạy được; bạn giải thích được một lỗi xuyên FE → API → DB.

## Những phần học sau khi đã có ứng dụng

[Generics nâng cao](phase-01-java-language/week-02/06-advanced-reference.md): wildcard/PECS/erasure và nested/anonymous class; [sealed type](phase-01-java-language/week-03/06-advanced-reference.md), tự viết JSON parser/LRU là nhánh đọc tiếp. [Concurrency chuyên sâu](phase-03-concurrency/README.md): custom thread pool, lock, CompletableFuture, structured concurrency. [JVM chuyên sâu](phase-04-jvm-performance/README.md): GC tuning, heap dump lab, reflection scanner/proxy. Chọn khi gặp vấn đề tương ứng; đây không phải điều kiện để bắt đầu Spring.

Kafka, Kubernetes, microservices và distributed systems chưa nằm trong đường học bắt buộc này.

## Nhịp 8 giờ mỗi tuần

Bốn buổi khoảng 2 giờ: (1) khái niệm + ví dụ nhỏ; (2) tự triển khai; (3) hoàn thiện và viết test; (4) debug, review, ghi lại 3 điều hiểu được và 1 điều còn vướng. Nếu mới với công cụ, dùng thời gian buổi 4 để thiết lập; giảm scope bài thay vì bỏ test.

Prompt review: “Tôi là FE 4 năm, mới học Java/BE. Đây là mục tiêu tuần, code và lỗi tôi đã thử sửa. Hãy chỉ ra tối đa 3 vấn đề quan trọng, giải thích bằng input/output cụ thể, hỏi tôi dự đoán kết quả trước khi đưa gợi ý. Chưa viết toàn bộ lời giải.”

## Nguồn tra cứu theo phiên bản

Java 21 là mốc để học; dùng patch JDK được cập nhật của nhà phân phối khi thực hành, không cần giữ patch cũ của ví dụ. Dùng [yêu cầu hệ thống Spring Boot](https://docs.spring.io/spring-boot/system-requirements.html) khi tạo project; Java 21 nằm trong dải tương thích được tài liệu hiện tại nêu. Ghi lại phiên bản đã chọn, theo tài liệu đúng phiên bản đó và để Boot quản lý dependency tương thích. Các snippet cũ là minh họa khái niệm, không phải project Spring hoàn chỉnh đã khóa phiên bản.
