# JIT, Garbage Collector Và GC Log

**Cách chạy ví dụ:** dùng JDK 21 (bộ công cụ Java). Mở terminal, gõ `jshell`; gõ từng khối Java độc lập, dùng `/reset` trước khối tiếp theo và `/exit` để thoát. JShell chạy được khai báo class, method (hàm của Java) và lệnh trực tiếp, không cần khung main. Các comment `//` giải thích dòng code và kết quả cần quan sát.

## JIT

JIT là cơ chế JVM tối ưu code được chạy nhiều lần trong lúc chương trình đang hoạt động.

**Quy tắc ngón tay cái:** Lần chạy đầu chưa đại diện tốc độ ổn định.

**Tự làm:** Đo cùng một việc nhiều lần và ghi sự biến động. **Gợi ý:** Không kết luận nguyên nhân chỉ từ thời gian.

## Benchmark Pitfalls

Benchmark là phép đo hiệu năng; phép đo nhỏ dễ bị ảnh hưởng bởi các công việc khác trên máy.

```java
long start = System.nanoTime(); // Mốc đo khoảng thời gian
long total = 0;
for (int i = 0; i < 10000; i++) total += i;
long elapsed = System.nanoTime() - start;
System.out.println(total); // 49995000: giữ kết quả có thể quan sát
System.out.println(elapsed); // Thời gian thay đổi theo lần chạy
```

**Khi nào dùng**

- Khi cần thao tác này trong bài thực hành.
- Khi đọc code có cùng cách viết.

**Lỗi hay gặp**

- Dùng trước khi hiểu kiểu đầu vào và kết quả.

**Quy tắc ngón tay cái:** Đúng kết quả trước, đo công bằng sau.

**Tự gõ, không dùng AI sinh code:** Chạy nhiều lần và ghi dải thời gian. **Gợi ý:** Ví dụ học cách đo, không dùng để tuyên bố Java nhanh/chậm.

## Garbage Collector

Garbage collector, viết tắt GC, thu hồi bộ nhớ object không còn cần truy cập theo các quy tắc JVM.

**Quy tắc ngón tay cái:** Bỏ reference không có nghĩa GC chạy ngay.

**Tự làm:** Giải thích vì sao list giữ mọi dữ liệu mãi có thể làm đầy heap. **Gợi ý:** GC không biết dữ liệu còn tham chiếu đó đã hết ích lợi nghiệp vụ.

## G1

G1 là một garbage collector cân bằng thời gian dừng và lượng công việc thu gom cho nhiều ứng dụng.

**Quy tắc ngón tay cái:** Giữ cấu hình mặc định trước khi có số đo.

**Tự làm:** Ghi collector hiện dùng của ứng dụng thực hành. **Gợi ý:** Đọc log khởi động; không đoán mọi JDK/môi trường có cùng mặc định.

## ZGC

ZGC là garbage collector hướng tới thời gian dừng ngắn.

**Quy tắc ngón tay cái:** Collector tốt hơn phụ thuộc yêu cầu và tải.

**Tự làm:** Nêu việc ứng dụng cần giảm thời gian dừng hay giảm bộ nhớ. **Gợi ý:** Chưa đổi collector chỉ vì tên mới hơn.

## GC Log

GC log là bản ghi hoạt động thu gom bộ nhớ. Với file Main.java đã chạy được, dùng `java '-Xlog:gc*' Main.java` trong thư mục chứa file. Dấu nháy giữ ký tự * nguyên vẹn trong zsh.

**Tự làm:** chạy ứng dụng nhỏ, tìm tên collector trong log. **Gợi ý:** không thấy lần thu gom không có nghĩa GC bị tắt.

## Bài Tập Nhanh

Chỉ quan sát trước khi tối ưu.

- Chạy một app nhỏ có log GC.
- Ghi tên collector và một dòng bạn hiểu.
- Giải thích giới hạn của phép đo thời gian.

**Gợi ý:** Giữ đầu vào và môi trường giống nhau khi so.

**Đạt khi:** Báo cáo có dữ liệu quan sát, không tuyên bố nguyên nhân chưa kiểm tra. Tự gõ phần triển khai, không dùng AI sinh lời giải.

## Đọc thêm sau

JMH là công cụ xây benchmark Java; dùng khi cần đo nghiêm túc. G1/ZGC tuning, allocation rate (tốc độ cấp phát) và pause distribution (phân bố thời gian dừng) học sau. [Java 21 GC guide](https://docs.oracle.com/en/java/javase/21/gctuning/).
