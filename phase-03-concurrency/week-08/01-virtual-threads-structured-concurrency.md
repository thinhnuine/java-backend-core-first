# Virtual Threads Và Structured Concurrency

> **Làm rõ trước khi học:** Ví dụ sleep chỉ mô phỏng chờ. Virtual thread không tự sửa race condition và không làm phép tính CPU chạy nhanh hơn.

**Cách chạy ví dụ:** dùng JDK 21 (bộ công cụ Java). Mở terminal, gõ `jshell`; gõ từng khối Java độc lập, dùng `/reset` trước khối tiếp theo và `/exit` để thoát. JShell chạy được khai báo class, method (hàm của Java) và lệnh trực tiếp, không cần khung main. Các comment `//` giải thích dòng code và kết quả cần quan sát.

## Virtual Threads

Virtual thread là luồng nhẹ do Java quản lý, giúp tổ chức nhiều công việc thường phải chờ đọc/ghi.

```java
try (var pool = java.util.concurrent.Executors.newVirtualThreadPerTaskExecutor()) {
    var result = pool.submit(() -> "done"); // Mỗi task dùng một virtual thread
    System.out.println(result.get()); // done
}
```

**Khi nào dùng**

- Nhiều công việc phải chờ đọc/ghi.
- Muốn giữ code chờ theo trình tự dễ đọc.

**Lỗi hay gặp**

- Nghĩ luồng nhẹ hơn tự tăng số kết nối DB.
- Nghĩ virtual thread sửa được race condition.

**Quy tắc ngón tay cái:** Nhiều việc chờ I/O: cân nhắc virtual thread, vẫn giới hạn tài nguyên bên ngoài.

**Tự gõ, không dùng AI sinh code:** Submit ba task trả tên khác nhau. **Gợi ý:** Chờ kết quả và để khối try đóng executor.

## Platform Thread Vs Virtual Thread

Platform thread gắn với luồng hệ điều hành; virtual thread được JVM (máy thực thi Java) bố trí lên luồng thực thi bên dưới.

**Quy tắc ngón tay cái:** Luồng nhẹ hơn không tạo thêm sức tính toán CPU.

**Tự làm:** Phân loại tải: chờ HTTP và tính tổng hàng triệu số. **Gợi ý:** CPU là bộ xử lý thực hiện phép tính; I/O là chờ đọc/ghi bên ngoài.

## Khi Nào Dùng Virtual Thread

Virtual thread hữu ích cho nhiều task thường chờ HTTP, file hoặc database (cơ sở dữ liệu).

**Quy tắc ngón tay cái:** Database có ít kết nối thì virtual thread vẫn phải chờ kết nối.

**Tự làm:** Vẽ 100 task cùng cần 5 kết nối DB. **Gợi ý:** Không suy ra 100 task là 100 thao tác DB đang chạy.

## Structured Concurrency

Structured concurrency gom các task con trong một phạm vi có vòng đời chung để dễ chờ và xử lý lỗi.

**Quy tắc ngón tay cái:** Java 21: virtual threads chính thức; StructuredTaskScope vẫn là preview (tính năng thử nghiệm).

**Tự làm:** Mô tả khi một trong hai tác vụ con lỗi thì tác vụ kia nên làm gì. **Gợi ý:** Chỉ nêu chính sách; chưa cần bật preview để học bài bắt buộc.

## Mental Model Cho FE Dev

Java có thể chạy nhiều luồng cùng sửa object; JS trên một luồng thường xen kẽ các callback qua event loop (vòng điều phối công việc).

**Quy tắc ngón tay cái:** Promise không phải virtual thread; đừng suy ra cơ chế chia sẻ dữ liệu giống nhau.

**Tự làm:** Chỉ ra một field dùng chung giữa hai request trong TaskService. **Gợi ý:** Dữ liệu riêng từng request nên đi qua tham số hoặc biến cục bộ.

## Bài Tập Nhanh

Thử executor trước khi đo tốc độ.

- Giao 10 task cho virtual thread executor.
- Lấy đủ kết quả.
- Đóng executor.

**Gợi ý:** Nếu dùng sleep để giả lập chờ, gọi đó là mô phỏng.

**Đạt khi:** Không mất kết quả và không kết luận đây là benchmark DB thật. Tự gõ phần triển khai, không dùng AI sinh lời giải.

## Đọc thêm sau

Pinning (carrier thread bị giữ khi virtual thread chặn trong một số tình huống Java 21) và giới hạn số thao tác bên ngoài học khi đo tải thật. [Virtual threads Java 21](https://docs.oracle.com/en/java/javase/21/core/virtual-threads.html), [StructuredTaskScope Java 21 preview](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/concurrent/StructuredTaskScope.html).
