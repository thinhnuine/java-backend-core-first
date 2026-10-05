# Lộ Trình Java Backend Core-First Cho FE Dev

## Tóm Tắt

Bạn học khoảng **8h/tuần**, đã có 4 năm kinh nghiệm FE với React/Next.js, nhưng muốn đi Java Backend theo hướng chắc nền trước khi dùng framework. Lộ trình này ưu tiên **Java core, JVM, concurrency, collections, testing và thiết kế code**, sau đó mới vào Spring Boot ở mức cốt lõi.

AI nên được dùng như mentor phụ: giải thích khái niệm, review code, tạo câu hỏi phản biện, gợi ý test case và giúp đọc stack trace. Phần bạn cần tự luyện là viết tay implementation, debug, đo hiệu năng, đọc tài liệu Java và tự giải thích lại quyết định thiết kế.

## Giai Đoạn 1: Ngôn Ngữ Java (Tuần 1-3)

Mục tiêu: viết Java đúng tinh thần Java, không chỉ port tư duy JS/TS sang cú pháp mới.

- Type system, primitive vs reference type, boxing/unboxing, null handling.
- OOP đúng kiểu Java: interface, abstract class, composition vs inheritance.
- `equals`, `hashCode`, `toString`, immutability.
- Generics: wildcard, bounded type, type erasure.
- `enum`, nested class, inner class, anonymous class.
- Java hiện đại 17/21: record, sealed class, pattern matching, switch expression, text block.
- Exception: checked vs unchecked, custom exception, try-with-resources.
- Maven project structure, package naming, basic dependency management.

Bài tập chính:

- Port một thư viện nhỏ từ JS/Python sang Java và tự viết tay implementation.
- Chọn một trong ba bài:
  - JSON parser đơn giản.
  - LRU cache.
  - Event emitter.
- Yêu cầu có README ngắn, unit test cơ bản, giải thích tradeoff về API design.

AI usage:

- Nhờ AI so sánh từng concept với TypeScript, ví dụ interface Java vs interface TypeScript.
- Nhờ AI review `equals/hashCode`, generic API và exception design.
- Không nhờ AI viết full implementation trước khi bạn tự thử.

## Giai Đoạn 2: Collections, Stream Và I/O (Tuần 4-5)

Mục tiêu: hiểu cấu trúc dữ liệu Java ở mức dùng đúng, đo được, và tránh lạm dụng Stream.

- Cấu trúc bên trong `ArrayList`, `HashMap`, `TreeMap`, `ConcurrentHashMap`.
- Độ phức tạp từng thao tác: get, put, remove, contains, iteration, sorting.
- Stream API, Collector, Optional.
- Functional interface, lambda, method reference.
- `java.time`.
- NIO.2: `Path`, `Files`, đọc/ghi file, directory traversal.
- Serialization ở mức nhận biết: Java serialization, JSON serialization, vì sao Java native serialization ít được dùng trong app hiện đại.

Bài tập chính:

- Xử lý file log lớn bằng cả Stream và vòng lặp thường.
- So sánh hai phiên bản về:
  - độ đọc code
  - memory usage
  - thời gian chạy
  - khả năng debug
- Viết report ngắn: khi nào dùng Stream, khi nào dùng loop.

AI usage:

- Nhờ AI review benchmark có công bằng không.
- Nhờ AI giải thích vì sao kết quả performance khác kỳ vọng.
- Nhờ AI gợi ý Collector hoặc data structure phù hợp, nhưng bạn tự quyết lựa chọn cuối.

## Giai Đoạn 3: Concurrency (Tuần 6-8)

Mục tiêu: hiểu concurrency đa luồng của Java, vì đây là phần khác JS nhiều nhất và dễ sai âm thầm.

- `Thread`, `Runnable`, thread lifecycle.
- `synchronized`, monitor lock, intrinsic lock.
- `volatile`, visibility, atomicity.
- Java Memory Model, happens-before.
- `ExecutorService`, thread pool, scheduling.
- `CompletableFuture`.
- `Lock`, `ReentrantLock`, `ReadWriteLock`.
- `Atomic*`, CAS ở mức khái niệm.
- `ConcurrentHashMap` trong bài toán concurrent access.
- Virtual threads và structured concurrency trong Java 21.
- Các lỗi kinh điển: race condition, deadlock, visibility bug, thread starvation.

Bài tập chính:

- Viết thread pool đơn giản.
- Viết producer-consumer queue.
- Viết rate limiter.
- Với mỗi bài, cố tình tạo bug concurrency rồi viết lại phiên bản đúng.

AI usage:

- Nhờ AI đóng vai reviewer tìm race condition.
- Nhờ AI giải thích bug bằng timeline nhiều thread.
- Nhờ AI tạo thêm test scenario, nhưng không tin test là đủ để chứng minh code concurrent đúng.

## Giai Đoạn 4: JVM Và Hiệu Năng (Tuần 9-10)

Mục tiêu: hiểu Java chạy như thế nào để sau này đọc được vấn đề production và hiểu Spring bớt "ma thuật".

- Cấu trúc bộ nhớ: heap, stack, metaspace.
- Class loading.
- JIT compiler ở mức khái niệm.
- Garbage collector: G1, ZGC.
- Đọc GC log cơ bản.
- Công cụ: JFR, VisualVM, `jcmd`, heap dump.
- Tìm memory leak trong app nhỏ.
- Reflection, annotation, dynamic proxy.
- Liên hệ với Spring: component scanning, annotation processing/runtime inspection, proxy cho transaction/security.

Bài tập chính:

- Tạo app nhỏ có memory leak có chủ đích, lấy heap dump và phân tích nguyên nhân.
- Dùng JFR hoặc VisualVM để quan sát CPU/memory/thread.
- Viết mini annotation + reflection scanner đơn giản.
- Viết dynamic proxy nhỏ để log method call.

AI usage:

- Nhờ AI giải thích GC log hoặc heap dump observation.
- Nhờ AI giúp liên hệ reflection/proxy với Spring AOP, transaction và DI.
- Nhờ AI đặt câu hỏi kiểm tra: "Nếu app leak memory thì em nhìn metric nào trước?"

## Giai Đoạn 5: Testing Và Thiết Kế Code (Tuần 11-12)

Mục tiêu: viết Java dễ test, dễ sửa, có style gần với codebase backend thật.

- JUnit 5.
- Mockito.
- AssertJ.
- Parameterized test.
- Test naming, test data, boundary case.
- SOLID ở mức thực dụng.
- Design pattern hay gặp trong Java: Builder, Strategy, Factory, Observer.
- Refactoring: tách responsibility, giảm coupling, đặt tên rõ, giảm mutation không cần thiết.
- Đọc **Effective Java** song song từ giai đoạn 1 đến giai đoạn 5.

Bài tập chính:

- Lấy lại một bài cũ như LRU cache, event emitter hoặc log processor để refactor.
- Thêm test coverage cho happy path, edge case và failure case.
- Áp dụng ít nhất một pattern có lý do rõ ràng, không dùng pattern để trang trí.

AI usage:

- Nhờ AI review test có đang test behavior hay chỉ test implementation.
- Nhờ AI chỉ ra code smell.
- Nhờ AI đề xuất refactor, sau đó bạn chọn một hướng và tự implement.

## Giai Đoạn 6: Spring Boot Ở Mức Cốt Lõi (Tuần 13-14)

Mục tiêu: dùng Spring Boot nhưng hiểu nó đang làm gì, không chỉ copy annotation.

- IoC/DI.
- Bean lifecycle.
- Autoconfiguration: hiểu nó được kích hoạt ra sao.
- REST API với controller/service/repository.
- DTO vs entity.
- Validation và global error handling.
- JPA/Hibernate cơ bản.
- N+1 query, lazy loading, transaction boundary.
- PostgreSQL với migration cơ bản bằng Flyway.
- Test REST API ở mức integration test.

Dự án cuối:

- Một REST API nhỏ, ví dụ Task API hoặc Notes API.
- Có CRUD, validation, error response, database persistence.
- Có ít nhất một quan hệ entity đơn giản.
- Có transaction ở service layer.
- Có integration test cho API chính.
- README giải thích cách chạy app và những phần Spring đang tự động làm.

AI usage:

- Nhờ AI giải thích autoconfiguration dựa trên dependency trong project.
- Nhờ AI review entity relationship, transaction boundary và lỗi N+1.
- Nhờ AI đọc stack trace Spring, nhưng bạn tự ghi lại nguyên nhân gốc bằng lời của mình.

## Lịch 8h/tuần

Mỗi tuần chia như sau:

- **2h học concept**: đọc tài liệu, sách, note, ví dụ nhỏ.
- **4h code bài tập**: tự viết implementation trước.
- **1h test/debug/benchmark**: viết test, đo hiệu năng, đọc lỗi.
- **1h AI review + learning log**: hỏi AI, ghi lại điều đã hiểu và câu hỏi còn mơ hồ.

Nhịp học gợi ý:

- Buổi 1: học concept và viết ví dụ nhỏ.
- Buổi 2: code bài tập chính.
- Buổi 3: test, debug, refactor hoặc benchmark.
- Buổi 4: review với AI, ghi chú, chốt checklist tuần.

## Nguyên Tắc Học Với AI

- Dùng AI như mentor, reviewer và người giải thích, không dùng như máy viết bài hộ.
- Trước khi hỏi lỗi, tự đọc stack trace và khoanh vùng file/class nghi ngờ.
- Với mỗi đoạn code AI gợi ý, bạn phải giải thích được:
  - Nó giải quyết vấn đề gì?
  - Input/output là gì?
  - Có thể fail ở đâu?
  - Test thế nào?
  - Có lựa chọn đơn giản hơn không?
- Mỗi tuần có ít nhất một phiên 60-90 phút tự code không dùng AI.
- Sau mỗi giai đoạn, viết một trang learning log: thứ đã hiểu, thứ còn yếu, bài tập đáng giữ trong portfolio.

## Tiêu Chí Đạt

Bạn coi một giai đoạn là đạt khi:

- Tuần 3: viết được một thư viện Java nhỏ có API rõ, có test và không copy từ AI.
- Tuần 5: giải thích được khi nào dùng `HashMap`, `TreeMap`, Stream, loop và xử lý file lớn.
- Tuần 8: nhận diện và sửa được race condition/deadlock/visibility bug trong bài tập nhỏ.
- Tuần 10: dùng được JFR/VisualVM/heap dump để điều tra vấn đề performance hoặc memory.
- Tuần 12: viết test tốt hơn, refactor code theo SOLID/pattern có lý do.
- Tuần 14: build được REST API Spring Boot nhỏ và giải thích được DI, transaction, JPA lazy loading, N+1.

## Giả Định

- Bạn học khoảng **8h/tuần**.
- Mục tiêu là chuyển sang Backend/fullstack Java với nền Java core chắc.
- Java version mặc định là **Java 21**, nhưng các concept nên hiểu tương thích với Java 17 vì nhiều công ty vẫn dùng 17.
- Build tool mặc định là **Maven**.
- Database mặc định ở giai đoạn Spring là **PostgreSQL**.
- Spring Boot chỉ bắt đầu sau khi đã học qua Java core, collections, concurrency, JVM và testing.
