# Mentor Guide Tuần 7: Executor, CompletableFuture, Lock Và Atomic

Học một ví dụ rồi dừng để tự sửa đầu vào. Các heading giữ nguyên để đối chiếu bản trước.

**Cách chạy ví dụ:** dùng JDK 21 (bộ công cụ Java). Mở terminal, gõ `jshell`; gõ từng khối Java độc lập, dùng `/reset` trước khối tiếp theo và `/exit` để thoát. JShell chạy được khai báo class, method (hàm của Java) và lệnh trực tiếp, không cần khung main. Các comment `//` giải thích dòng code và kết quả cần quan sát.

## 1. Ý chính

Future giữ kết quả hoặc lỗi của task (đơn vị công việc).

## 2. Giải thích code

```java
try (var pool = java.util.concurrent.Executors.newFixedThreadPool(2)) {
    var result = pool.submit(() -> 21 * 2); // Giao việc và giữ kết quả tương lai
    System.out.println(result.get()); // Chờ rồi in 42
} // Java 21: đóng pool và chờ việc đã giao kết thúc
```

## 3. Vì sao thiết kế như vậy

Giao việc và chờ kết quả là hai thao tác riêng. **Quy tắc ngón tay cái:** tự giải thích được output trước khi mở rộng bài.

## 4. Liên hệ với Frontend

Future gần Promise về ý tưởng kết quả tương lai. get có thể chặn thread đang gọi, không giống await nhường điều khiển về event loop.

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

**Tự gõ, không dùng AI sinh code:** Thay một task bằng phép tính ném lỗi rồi đọc nguyên nhân.

**Gợi ý:** Dùng getCause trên ExecutionException.

## Đọc thêm sau

CompletableFuture phức tạp, khóa nhiều tầng, LongAdder và tải cao. Đây là phần tự chọn. [Quay lại trang module](README.md).
