# Từ Chạy Trong IDE Đến Chạy Lại Được

Học tuần 19–21 mới. Cần API dùng DB và test chạy được. Chia thành ba buổi/chặng: JAR/config → container/network/volume → CI và môi trường demo. Mỗi chặng còn lại dùng thời gian thực hành và debug theo roadmap.

## 1. Ý chính

Build tạo artifact để chạy ứng dụng. Runtime cần cấu hình môi trường như URL database và credential. Container đóng gói process cùng môi trường cần để chạy; CI tự động chạy các bước kiểm tra khi code đổi. Bạn vẫn phải thiết lập network và lưu trữ dữ liệu có chủ đích.

## 2. Giải thích lệnh và luồng

Từ Spring project có Maven Wrapper và Boot packaging plugin:

```sh
./mvnw verify
./mvnw package
```

verify chạy các bước đã cấu hình; package tạo JAR trong target. Lệnh package cũng có các bước test trước đó nếu không bị skip. Khi thử manual, chỉ cần chạy verify một lượt rồi dùng artifact đã được tạo; không cần lặp mọi lệnh mỗi lần.

Xem tên JAR thực tế, rồi chạy (thay tên bên dưới):

```sh
java -jar target/task-api-0.0.1-SNAPSHOT.jar
```

JAR cần là executable JAR do Boot plugin tạo, không phải bất kỳ JAR nào. Chạy ngoài IDE cho biết bạn có phụ thuộc cấu hình riêng của IDE hay không.

Snippet application.properties trong Spring project:

```properties
spring.datasource.url=${TASK_DB_URL}
spring.datasource.username=${TASK_DB_USER}
spring.datasource.password=${TASK_DB_PASSWORD}
spring.jpa.hibernate.ddl-auto=validate
```

Đặt biến môi trường bằng cấu hình run của terminal/container/hosting. Commit file mẫu chỉ có tên biến và giá trị giả; credential thật ở môi trường chạy. validate kiểm mapping/schema; Flyway chịu trách nhiệm thay schema. Thiếu DB URL hoặc migration lỗi phải được phát hiện và chẩn đoán từ log.

Để học Docker, theo [container basics](https://docs.docker.com/get-started/docker-concepts/the-basics/what-is-a-container/) rồi tự tạo Dockerfile cho JAR đã build. Xác định base image Java 21, copy đúng artifact, lệnh java -jar, port và env. Ghi version/digest image dùng cho demo vào README; các tag chung có thể trỏ đến nội dung mới theo thời gian.

Ba khái niệm trước khi ghép container:

| Khái niệm | Ví dụ cần hiểu |
| --- | --- |
| Port mapping | Host 8080 được chuyển vào port API trong container |
| Network | Trong container API, localhost là chính container đó; DB ở container khác cần hostname/service name của DB |
| Volume | PostgreSQL data ở volume riêng để tồn tại khi thay container, theo [Docker persistence guide](https://docs.docker.com/get-started/docker-concepts/running-containers/persisting-container-data/) |

Chạy API/DB bằng Compose hoặc cách tương đương, theo tài liệu đúng phiên bản image DB. Không suy ra đường dẫn volume PostgreSQL chỉ từ một tutorial dùng major khác. README cần chỉ rõ thao tác restart bình thường và thao tác xóa dữ liệu; không dùng reset DB như bước mặc định mỗi lần chạy.

CI tối thiểu: checkout → thiết lập Java 21 → dựng DB test riêng nếu test cần → ./mvnw verify → build FE → lưu report khi fail. Chọn nền tảng CI của repo; đối chiếu syntax/action phiên bản từ tài liệu chính thức lúc tạo pipeline.

## 3. Vì sao thiết kế như vậy

Mẫu sai về test: đặt tên TaskApiIT rồi chạy mvn test và kết luận integration test đã pass. Surefire/Failsafe có convention và lifecycle khác nhau; phải kiểm report thật.

Nếu chọn tên *IT, thêm Failsafe với hai goal integration-test và verify, chạy ./mvnw verify. Nếu chọn *Test và chạy bằng Surefire, dùng convention đó nhất quán. Theo [Failsafe usage](https://maven.apache.org/surefire/maven-failsafe-plugin/usage.html), gọi verify để hoàn tất các bước kiểm tra/cleanup phù hợp thay vì chỉ dừng ở integration-test.

## 4. Liên hệ với frontend

Build FE tạo bundle; build API tạo artifact chạy phía server. API environment secret phải được giữ ở server; biến public trong FE bundle không phù hợp để giữ DB credential. Browser gọi địa chỉ công khai/proxy; nó không sử dụng hostname riêng của Compose network như API gọi DB.

## 5. Khi nào dùng và không dùng

Dùng container để chạy lại môi trường và CI cho kiểm tra lặp. Trước khi thêm container, chạy JAR bằng tay để dễ tách lỗi code/config khỏi lỗi network. Hosting trả phí là lựa chọn; nghiệm thu có thể dùng local có README rõ. Một demo chạy được chưa thay thế backup, giám sát và kinh nghiệm production.

## 6. Bẫy hay gặp

localhost sai context; volume mount sai theo DB major; chỉ thấy process còn sống rồi kết luận DB sẵn sàng; test dùng DB demo; test bị skip nhưng CI xanh; gửi credential/encoded password vào log; restart API rồi tưởng session in-memory bắt buộc còn tồn tại.

Health kiểm tình trạng tổng quát; readiness trả lời app có thể phục vụ theo điều kiện đã chọn. Thử tắt DB và quan sát; endpoint quản trị chỉ công khai phần cần thiết. Timeout cần cấu hình theo layer: connection pool, DB driver/query, HTTP client; một timeout không tự bao mọi thao tác.

## 7. Thuật ngữ mới

Artifact là kết quả build; image là mẫu môi trường/container; container là process đang chạy từ image; volume là lưu trữ quản lý riêng; CI là quy trình tự động kiểm tra khi code đổi; readiness là khả năng sẵn sàng phục vụ theo cấu hình.

## 8. Bài tập nhỏ để tự làm

1. Chạy API từ JAR bằng biến môi trường, gọi một endpoint có DB.
2. Chạy bằng container, nối DB và ghi dữ liệu; restart bình thường rồi kiểm dữ liệu vẫn còn.
3. Tắt DB bài tập, lần dấu lỗi từ FE response tới API log; khôi phục và xác minh phục hồi.
4. Làm một test fail có chủ đích, kiểm CI fail đúng; khôi phục rồi kiểm pass và số test thực sự chạy.
5. Đưa README cho một người hoặc dùng môi trường sạch để chạy lại, ghi bước thiếu và bổ sung.

Đạt khi tự phân biệt lỗi ứng dụng, config, network và DB, đồng thời chứng minh dữ liệu/test hoạt động đúng. Tiếp theo: [bài nghiệm thu cuối](../COURSE_OUTCOMES.md), đối chiếu [project chặng F](03-task-manager-project.md).
