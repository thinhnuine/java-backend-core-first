# Review Toàn Bộ Giáo Trình Cho FE Mới Học Java/Backend

## Kết luận sau lượt rà toàn bộ

Đường học bắt buộc phù hợp hơn với mục tiêu fullstack sau khi đổi thứ tự, chia nhỏ Java cơ bản, bổ sung HTTP/SQL và đưa testing lên sớm. Bộ bài vẫn có hai loại: bài hướng dẫn nền để tự chạy từng bước và bài tham khảo nâng cao dùng khi đã có prerequisites. Không dùng độ dài hoặc số file để xác định “đã học xong”.

Lượt rà này xét bài giảng, bài tập, checklist, mentor guide, trang điều hướng và project trong cả sáu phase cùng learning-path. Sau bổ sung, bộ tài liệu có 107 file Markdown; đã kiểm tra 294 liên kết nội bộ. Đã sửa những lỗi/điểm gây hiểu nhầm phát hiện được và đối chiếu các behavior quan trọng với tài liệu chính thức. Đây là review tài liệu; không phải xác nhận đã dựng mọi project tự chọn, chạy mọi snippet framework hay benchmark tất cả bài trên môi trường thật.

## Đánh giá từng module

| Module cũ | Đánh giá và phạm vi theo lịch mới | Thay đổi cần thiết đã thực hiện |
| --- | --- | --- |
| week-01 | Chi tiết, nhưng rất dài; học từng phần tuần 2–4 | Giảm yêu cầu trang vào; làm rõ field sketch chưa compile, interface method và Money unit/overflow |
| week-02 | Bài cơ bản dễ tiếp cận hơn sau lần viết lại | Giữ Box/enum ở lượt đầu; guide/repository/LRU là đọc thêm |
| week-03 | Đủ bước vào record/switch/exception | Checklist tập trung bài Task nhỏ; guide cũ có chú thích shallow immutability và exception hierarchy |
| week-04 | Internals có ích nhưng không nên là bài đầu collection | Thêm ví dụ List/Set/Map chạy được; sửa bảng key/value; tách benchmark/concurrent word counter thành mở rộng |
| week-05 | Pipeline/Optional cần ví dụ tự chứa và so với loop | Viết lại ví dụ đầy đủ; giải thích lazy/terminal/toList/get; giữ collector/log processor làm tra cứu |
| week-06 | Cần một phần nhỏ khi API bắt đầu nhận request đồng thời | Thêm giới hạn demo race/visibility, join/interruption; không dùng vài lượt chạy như chứng minh an toàn |
| week-07 | Học sâu sau project | Sửa hiểu nhầm get làm mất song song, fixed pool queue/timeout/shutdown; yêu cầu lifecycle rõ |
| week-08 | Phù hợp mở rộng khi đã biết executor/lock | Chốt preview ở Java 21; giới hạn DB quota/pinning; sleep demo không thay benchmark I/O |
| week-09 | Heap/stack/classpath có ích ở mức căn bản | Phân biệt String pool với vùng memory; quote GC log option cho zsh; GC/JIT lab là mở rộng |
| week-10 | Tooling/annotation/proxy là tra cứu theo vấn đề | Sửa leak demo để process còn sống khi lấy dump; thêm import/setup; giới hạn mental model transaction |
| week-11 | Cần học JUnit sớm, mở rộng test khi có API/DB | Sửa mock void charge và assertion Money; chỉ rõ dependency/import/null input; ưu tiên test Task Manager |
| week-12 | Tư duy thực dụng tốt | Ưu tiên refactor TaskService, bỏ lời gọi Money.divide chưa tồn tại; làm rõ currency của sample strategy |
| week-13 | Dàn ý cũ thiếu cửa vào Spring | Viết lại setup, application/service/controller, lệnh chạy và output; làm rõ validation/security error; yêu cầu HTTP test |
| week-14 | Cần học dần trong tuần 12–15 | Bổ sung rename/no-arg constructor và giới hạn schema; làm rõ JPA/flush/commit/N+1; nghiệm thu bằng PostgreSQL |

## Những vấn đề kỹ thuật đã sửa

- **Collection:** tách HashMap.containsKey/get khỏi containsValue; không gộp độ phức tạp Map/Set dưới tên “contains value”. Bài mutable key giữ equals/hashCode nhất quán để chỉ quan sát đúng một bug. [HashMap API](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/HashMap.html).
- **Stream/Optional:** thêm lazy/terminal, stream chỉ tiêu thụ một lần, list không sửa cấu trúc của Stream.toList, và Optional.get vẫn có thể lỗi runtime. [Stream API](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/stream/Stream.html).
- **Concurrency:** fixed pool giới hạn worker nhưng queue mặc định không giới hạn; get(timeout) không mặc nhiên hủy task; interruption cần được xử lý. Việc get hai future đã submit không làm hai task đó mất khả năng chạy song song. [Executors API](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/concurrent/Executors.html).
- **Tính nhất quán giữa bài:** PaymentGateway của bài OOP trả void nên không stub thenReturn; mẫu Money chưa có equality thì test kiểm field; mẫu chưa có divide không được gọi như API sẵn có.
- **Spring/JPA:** snippet thiếu method/constructor hoặc chưa khớp full schema được chỉ rõ. Proxy/rollback rules, query/transaction và filter-chain errors được giải thích để tránh mặc định sai. [Spring transaction reference](https://docs.spring.io/spring-framework/reference/data-access/transaction/declarative/annotations.html).
- **Tooling:** lệnh -Xlog:gc* được quote để zsh không expand glob; demo heap retention có thời gian lấy dump trước khi process kết thúc.

## Những khoảng trống về cách dạy đã bổ sung

1. Bài List/Set/Map chạy được trước khi đọc internals.
2. Bài Stream dùng cùng input cho loop và pipeline, mỗi bước có output để đối chiếu.
3. Bài bắt đầu Spring có project setup, package, application class và endpoint nhỏ trước Task API.
4. Bài session/ownership/CORS/CSRF làm rõ luồng login và cách kiểm quyền bằng hai tài khoản.
5. Bài JAR/config/container/network/volume/CI làm rõ cách chạy lại app và kiểm test thật sự được thực thi.
6. Chuẩn đầu ra riêng với bài thêm tính năng và các case thất bại phải chứng minh.

Đã bỏ lịch 8h cũ khỏi các trang vào module còn gây nhầm. Tổng một tuần mới là 8 giờ cho các phần được chọn, không phải 8 giờ mỗi module. Các bài tập/checklist nâng cao không tự trở thành điều kiện để bắt đầu Spring.

## Độ nặng của lộ trình

24 tuần × 8 giờ là kế hoạch ban đầu. Tuần 4–5 có nhiều khái niệm; SQL/JPA, auth và Docker có công cụ mới. Dự phòng thêm 4–8 tuần củng cố là ước lượng để học thực chất, không phải thời lượng đã đo trên người học. Làm tối thiểu Task Manager trước, chọn bài sâu sau theo nhu cầu.

## Mục tiêu và đánh giá sau khóa

Xem [COURSE_OUTCOMES.md](COURSE_OUTCOMES.md): tự triển khai, kiểm thử, giải thích và sửa một tính năng xuyên FE → API → DB. Cần có code chạy được, case lỗi được kiểm tra và giải thích độc lập. Giao diện đẹp hoặc đọc hết tài liệu không bù được lỗi kiểm quyền/dữ liệu.

Điểm đến là nền tảng làm ứng dụng nhỏ và tham gia việc fullstack có review. Java backend production ở quy mô lớn, performance tuning, distributed systems và chức danh senior cần thêm thực hành nghề nghiệp.

## Phạm vi kiểm chứng

Ví dụ Java nền được kiểm tra compile/run, các case lỗi liên quan được thử có chủ đích; liên kết Markdown và định dạng code block được kiểm tra. Các bài framework yêu cầu project/dependency đúng phiên bản và phải kiểm chứng khi thực hành. Không báo cáo test API/DB/security/container đã pass nếu chưa dựng và chạy các hệ đó.

Tiếp theo: [roadmap](ROADMAP.md) để chọn bài theo tuần, [Task Manager](learning-path/03-task-manager-project.md) để biết đầu ra thực hành.
