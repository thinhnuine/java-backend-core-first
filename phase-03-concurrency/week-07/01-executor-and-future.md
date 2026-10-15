# ExecutorService Và Future

> **Làm rõ trước khi học:** newFixedThreadPool dùng hàng đợi không giới hạn mặc định. get(timeout) chỉ giới hạn lúc chờ; cancel(true) yêu cầu ngắt chứ không cưỡng bức mọi task dừng.

**Cách chạy ví dụ:** dùng JDK 21 (bộ công cụ Java). Mở terminal, gõ `jshell`; gõ từng khối Java độc lập, dùng `/reset` trước khối tiếp theo và `/exit` để thoát. JShell chạy được khai báo class, method (hàm của Java) và lệnh trực tiếp, không cần khung main. Các comment `//` giải thích dòng code và kết quả cần quan sát.

## Vì Sao Không Tự Tạo Thread Mãi

ExecutorService là công cụ nhận việc và quản lý các luồng chạy việc đó, giúp bạn không phải tự tạo và theo dõi từng thread.

**Quy tắc ngón tay cái:** Giao việc cho executor, xác định ai đóng nó.

**Tự làm:** Liệt kê nơi tạo và nơi kết thúc executor trong bài của bạn. **Gợi ý:** Một task là một đơn vị công việc, không nhất thiết một thread mới.

## Fixed Thread Pool

Fixed thread pool là nhóm có số luồng xử lý cố định, dùng lại để chạy nhiều task.

```java
try (var pool = java.util.concurrent.Executors.newFixedThreadPool(2)) {
    var result = pool.submit(() -> 21 * 2); // Giao việc và giữ kết quả tương lai
    System.out.println(result.get()); // Chờ rồi in 42
} // Java 21: đóng pool và chờ việc đã giao kết thúc
```

**Khi nào dùng**

- Giới hạn số worker (luồng làm việc).
- Thực hiện nhiều task độc lập.

**Lỗi hay gặp**

- Nghĩ số task chờ cũng được giới hạn tự động.

**Quy tắc ngón tay cái:** Giới hạn worker không đồng nghĩa giới hạn hàng đợi.

**Tự gõ, không dùng AI sinh code:** Giao hai phép tính rồi lấy cả hai kết quả. **Gợi ý:** Submit cả hai trước khi get để chúng có cơ hội chạy cùng lúc.

## `Future`

Future là object giữ kết quả đang hoặc sẽ được tính; get chờ kết quả khi nó chưa sẵn sàng.

```java
try (var pool = java.util.concurrent.Executors.newFixedThreadPool(2)) {
    var result = pool.submit(() -> 21 * 2); // Giao việc và giữ kết quả tương lai
    System.out.println(result.get()); // Chờ rồi in 42
} // Java 21: đóng pool và chờ việc đã giao kết thúc
```

**Khi nào dùng**

- Chờ kết quả đã giao cho executor.
- Kiểm trạng thái hoàn thành của task.

**Lỗi hay gặp**

- Nghĩ get luôn trả ngay.
- Nghĩ get hai task đã submit làm chúng mất khả năng chạy song song.

**Tự gõ, không dùng AI sinh code:** Đổi kết quả thành String rồi đọc nó. **Gợi ý:** Kiểu future đi theo giá trị method submit trả về.

## Shutdown Đúng

Shutdown là dừng nhận việc mới và quản lý việc còn lại trước khi ứng dụng kết thúc.

**Quy tắc ngón tay cái:** Bài ngắn Java 21: dùng try-with-resources cho executor.

**Tự làm:** Chỉ ra dấu } đóng executor ở ví dụ pool. **Gợi ý:** Close chờ task kết thúc; task treo vẫn có thể làm close chờ mãi.

## Exception Trong Task

Lỗi trong task được Future chuyển ra khi bạn gọi get, để bên gọi quyết định xử lý.

```java
try (var pool = java.util.concurrent.Executors.newSingleThreadExecutor()) {
    var result = pool.submit(() -> { throw new IllegalArgumentException("bad input"); });
    try { result.get(); }
    catch (java.util.concurrent.ExecutionException error) {
        System.out.println(error.getCause().getMessage()); // bad input: lỗi gốc
    }
}
```

**Khi nào dùng**

- Task có thể thất bại.
- Caller cần xử lý lỗi gốc.

**Lỗi hay gặp**

- Chỉ xem thông báo lỗi bọc, bỏ qua cause.
- Giao task rồi không quan sát lỗi của nó.

**Tự gõ, không dùng AI sinh code:** Giao một task thành công và một task lỗi, quan sát riêng hai kết quả. **Gợi ý:** getCause lấy lỗi gốc bên trong lỗi bọc.

## Bài Tập Nhanh

Tạo chương trình giao ba phép tính độc lập.

- Submit ba task.
- Lấy kết quả theo thứ tự input.
- Đóng executor khi xong.

**Gợi ý:** Giữ danh sách Future thay vì get ngay sau từng submit.

**Đạt khi:** Kết quả đúng và chương trình kết thúc được. Tự gõ phần triển khai, không dùng AI sinh lời giải.

## Queue, timeout và interruption

Queue là hàng đợi task; timeout là giới hạn thời gian chờ; interruption là yêu cầu hợp tác dừng việc.

**Quy tắc ngón tay cái:** Hết thời gian chờ không có nghĩa công việc bên dưới đã dừng.

**Tự làm:** Vẽ hai mốc: caller ngừng chờ và task thực sự kết thúc. **Gợi ý:** Đặt timeout ở thao tác I/O (đọc/ghi bên ngoài) khi cần, không chỉ ở Future.

## Đọc thêm sau

ThreadPoolExecutor với hàng đợi có giới hạn, chính sách từ chối task và shutdown có deadline để sau. [Executors Java 21](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/concurrent/Executors.html).
