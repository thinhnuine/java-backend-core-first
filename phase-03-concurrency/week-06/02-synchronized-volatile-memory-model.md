# `synchronized`, `volatile` Và Java Memory Model

**Cách chạy ví dụ:** dùng JDK 21 (bộ công cụ Java). Mở terminal, gõ `jshell`; gõ từng khối Java độc lập, dùng `/reset` trước khối tiếp theo và `/exit` để thoát. JShell chạy được khai báo class, method (hàm của Java) và lệnh trực tiếp, không cần khung main. Các comment `//` giải thích dòng code và kết quả cần quan sát.

## `synchronized`

synchronized khóa một vùng thao tác trên cùng object để mỗi lần chỉ một thread thực hiện vùng đó.

```java
class Counter {
    private int value;
    synchronized void increment() { value++; } // Khóa object Counter này
    synchronized int value() { return value; }
}
Counter counter = new Counter(); counter.increment();
System.out.println(counter.value()); // 1
```

**Khi nào dùng**

- Nhiều luồng sửa cùng trạng thái.
- Cần bảo vệ một nhóm thao tác cùng nhau.

**Lỗi hay gặp**

- Mỗi thread khóa một object khác nhau.
- Đọc field dùng chung qua lối không đồng bộ.

**Quy tắc ngón tay cái:** Cùng dữ liệu, cùng khóa, mọi lối truy cập phải nhất quán.

**Tự gõ, không dùng AI sinh code:** Thay UnsafeCounter bằng Counter và chạy hai thread. **Gợi ý:** Cả hai thread phải dùng cùng một counter.

## `volatile`

volatile giúp truyền giá trị mới giữa các luồng qua một field; nó không gộp nhiều thao tác thành một.

```java
class Flag { volatile boolean ready; }
Flag flag = new Flag();
Thread worker = new Thread(() -> flag.ready = true); // Ghi flag
worker.start(); worker.join();
System.out.println(flag.ready); // true; join cũng bảo đảm thấy kết quả
```

**Khi nào dùng**

- Cờ báo trạng thái đơn giản.
- Truyền một giá trị cập nhật giữa luồng.

**Lỗi hay gặp**

- Nghĩ volatile làm count++ atomic (không bị xen ngang).

**Quy tắc ngón tay cái:** volatile cho nhìn thấy, không thay khóa cho phép tăng nhiều bước.

**Tự gõ, không dùng AI sinh code:** Giải thích vì sao thêm volatile vào count++ vẫn chưa đủ. **Gợi ý:** Tách count++ thành ba bước; volatile không khóa ba bước đó.

## Java Memory Model

Java Memory Model là bộ quy tắc nói khi nào một luồng được bảo đảm thấy dữ liệu do luồng khác ghi.

**Quy tắc ngón tay cái:** Dựa vào quy tắc đồng bộ, không dựa vào “máy mình luôn chạy đúng”.

**Tự làm:** Tìm start, join hoặc synchronized trong ví dụ của bạn. **Gợi ý:** Nói rõ chúng kết nối hai luồng nào; mô hình CPU/cache để sau.

## Happens-Before Hay Gặp

Happens-before là quan hệ bảo đảm thứ tự và khả năng thấy dữ liệu giữa thao tác. start, join và cùng khóa synchronized tạo các quan hệ thường dùng.

**Quy tắc ngón tay cái:** Đợi join thành công rồi mới đọc kết quả worker.

**Tự làm:** Đánh dấu điểm đọc count sau join trong bài counter. **Gợi ý:** Bỏ join có thể khiến bạn đọc khi worker chưa xong, ngoài lỗi cập nhật.

## `wait/notifyAll`

wait cho luồng tạm nhường khóa để chờ điều kiện; notifyAll đánh thức các luồng đang chờ cùng object để kiểm tra lại điều kiện.

**Quy tắc ngón tay cái:** Chờ điều kiện trong while, không chỉ if.

**Tự làm:** Vẽ luồng lấy hàng khi hàng đợi đang rỗng. **Gợi ý:** Ưu tiên dùng BlockingQueue (hàng đợi hỗ trợ chờ) trước khi tự viết bằng wait.

## Bài Tập Nhanh

Sửa một lỗi trước khi học mọi cơ chế đồng bộ.

- Dùng synchronized cho counter.
- Vẽ vùng đọc-cộng-ghi được bảo vệ.
- Thử bản volatile và giải thích giới hạn.

**Gợi ý:** Chạy nhiều lần là quan sát bổ sung, không thay lập luận.

**Đạt khi:** Nêu được object khóa nào bảo vệ field nào. Tự gõ phần triển khai, không dùng AI sinh lời giải.

## Đọc thêm sau

wait/notifyAll chỉ gọi khi giữ khóa của đúng object; while cần vì điều kiện có thể không còn đúng sau khi thức dậy. InterruptedException báo yêu cầu ngắt lúc chờ; xử lý bằng truyền tiếp hoặc khôi phục cờ ngắt rồi kết thúc phù hợp. [JLS 21: memory model](https://docs.oracle.com/javase/specs/jls/se21/html/jls-17.html).
