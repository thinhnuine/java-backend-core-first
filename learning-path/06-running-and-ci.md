# Từ Chạy Trong IDE Đến Chạy Lại Được

Học một ví dụ rồi dừng để tự sửa đầu vào. Các heading giữ nguyên để đối chiếu bản trước.

**Cách chạy ví dụ:** dùng JDK 21 (bộ công cụ Java). Mở terminal, gõ `jshell`; gõ từng khối Java độc lập, dùng `/reset` trước khối tiếp theo và `/exit` để thoát. JShell chạy được khai báo class, method (hàm của Java) và lệnh trực tiếp, không cần khung main. Các comment `//` giải thích dòng code và kết quả cần quan sát.

## 1. Ý chính

Build tạo bản ứng dụng để chạy; CI là quy trình tự động chạy kiểm tra khi code đổi, giúp bạn phát hiện lỗi trước khi đưa cho người khác.

## 2. Giải thích lệnh và luồng

```java
String mode = System.getenv("TASK_MODE"); // Đọc biến môi trường từ hệ điều hành
System.out.println(mode == null ? "local" : mode); // Có giá trị mặc định
```

## 3. Vì sao thiết kế như vậy

Config là cấu hình thay theo nơi chạy; tách nó khỏi code giúp dùng cùng bản ứng dụng ở nhiều môi trường. **Quy tắc ngón tay cái:** chạy ngoài IDE trước, đóng gói môi trường sau.

## 4. Liên hệ với frontend

FE build tạo bundle; Java build có thể tạo JAR (file đóng gói code). Biến public đưa vào FE bundle không dùng để giữ mật khẩu database.

## 5. Khi nào dùng và không dùng

**Khi nào dùng**

- Đưa project cho người khác chạy lại.
- Kiểm code bằng quy trình lặp tự động.

## 6. Bẫy hay gặp

**Lỗi hay gặp**

- Commit mật khẩu thật vào Git.
- CI xanh nhưng test bị bỏ qua.
- Trong container gọi localhost và tưởng đang gọi DB ở container khác.

## 7. Thuật ngữ mới

**Artifact**: sản phẩm build. **Container**: môi trường chạy process được đóng gói. **Image**: mẫu tạo container. **Volume**: nơi lưu dữ liệu độc lập với vòng đời container.

## 8. Bài tập nhỏ để tự làm

**Tự gõ, không dùng AI sinh code:** Chạy API ngoài IDE theo README rồi kiểm dữ liệu còn sau restart.

**Gợi ý:** Với Spring project có Maven Wrapper, chạy ./mvnw verify rồi java -jar với đúng tên executable JAR trong target.

## Đọc thêm sau

### Khi cần Docker và CI

Chạy JAR trước; sau đó tạo image Java 21, nối API với DB bằng hostname đúng và giữ dữ liệu ở volume. Lệnh verify chỉ chạy những kiểm tra đã cấu hình: test *IT cần Failsafe hoặc quy ước khác được cấu hình rõ.

CI tối thiểu: lấy code → Java 21 → database test nếu cần → verify → build FE → lưu report khi lỗi. Thử một test fail để biết pipeline thực sự kiểm tra. [Docker basics](https://docs.docker.com/get-started/docker-concepts/the-basics/what-is-a-container/), [Maven Failsafe usage](https://maven.apache.org/surefire/maven-failsafe-plugin/usage.html). Health/readiness (trạng thái sống/sẵn sàng), timeout từng tầng và vận hành nâng cao học sau.
