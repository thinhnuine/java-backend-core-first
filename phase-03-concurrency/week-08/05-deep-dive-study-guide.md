# Mentor Guide Tuần 8: Virtual Threads Và Concurrency Capstone

Học một ví dụ rồi dừng để tự sửa đầu vào. Các heading giữ nguyên để đối chiếu bản trước.

**Cách chạy ví dụ:** dùng JDK 21 (bộ công cụ Java). Mở terminal, gõ `jshell`; gõ từng khối Java độc lập, dùng `/reset` trước khối tiếp theo và `/exit` để thoát. JShell chạy được khai báo class, method (hàm của Java) và lệnh trực tiếp, không cần khung main. Các comment `//` giải thích dòng code và kết quả cần quan sát.

## 1. Ý chính

Virtual thread là luồng nhẹ; executor quản lý lúc giao việc và lúc kết thúc.

## 2. Giải thích code

```java
try (var executor = java.util.concurrent.Executors.newVirtualThreadPerTaskExecutor()) {
    var answer = executor.submit(() -> 42);
    System.out.println(answer.get()); // 42
} // Đóng sau khi công việc xong
```

## 3. Vì sao thiết kế như vậy

Luồng rẻ hơn không làm mọi tài nguyên khác vô hạn. **Quy tắc ngón tay cái:** tự giải thích được output trước khi mở rộng bài.

## 4. Liên hệ với Frontend

Bạn quen chờ I/O bằng Promise; virtual thread cho phép viết code chờ theo trình tự. Nhiều Java thread vẫn có thể sửa chung dữ liệu.

## 5. Khi nào dùng và không dùng

**Khi nào dùng**

- Khi cần củng cố sau bài chính.
- Khi gặp đúng tình huống ví dụ trong code của bạn.

## 6. Bẫy hay gặp

**Lỗi hay gặp**

- Chép output mà chưa tự chạy.
- Mở rộng nhiều khái niệm cùng lúc.

## 7. Thuật ngữ mới

Các tên API trong ví dụ chỉ phục vụ một thao tác đang học. Nếu chưa biết class/method, quay lại bài cơ bản được dẫn trong README module.

## 8. Bài tập nhỏ để tự gõ lại

**Tự gõ, không dùng AI sinh code:** Nêu giới hạn kết nối DB khi nhiều task cùng gọi DB.

**Gợi ý:** Đếm tài nguyên bên ngoài, không chỉ đếm thread.

## Đọc thêm sau

Structured concurrency preview, pool tự viết và rate limiter. Đây là phần tự chọn. [Quay lại trang module](README.md).
