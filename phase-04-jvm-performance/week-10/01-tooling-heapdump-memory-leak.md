# Tooling, Heap Dump Và Memory Leak

**Cách chạy ví dụ:** dùng JDK 21 (bộ công cụ Java). Mở terminal, gõ `jshell`; gõ từng khối Java độc lập, dùng `/reset` trước khối tiếp theo và `/exit` để thoát. JShell chạy được khai báo class, method (hàm của Java) và lệnh trực tiếp, không cần khung main. Các comment `//` giải thích dòng code và kết quả cần quan sát.

## Công Cụ

JFR ghi sự kiện lúc ứng dụng chạy; jcmd gửi lệnh chẩn đoán tới JVM; VisualVM hiển thị dữ liệu để tìm lỗi.

**Quy tắc ngón tay cái:** Chọn câu hỏi trước, chọn công cụ sau.

**Tự làm:** Viết một câu hỏi như “object nào giữ nhiều dữ liệu?”. **Gợi ý:** Không mở mọi biểu đồ cùng lúc rồi đoán.

## Lệnh `jcmd`

`jcmd -l` liệt kê JVM có thể truy cập. PID là mã tiến trình; thay `<pid>` bằng mã thật rồi dùng `jcmd <pid> GC.heap_info` để đọc thông tin heap.

**Tự làm:** tìm PID ứng dụng lab đang chạy. **Gợi ý:** chạy công cụ bằng cùng người dùng; không chọn một process không thuộc bài.

## Memory Leak Trong Java

Memory leak là dữ liệu hết cần theo nghiệp vụ nhưng vẫn được giữ lại, khiến bộ nhớ tăng ngoài dự định.

**Quy tắc ngón tay cái:** GC không thu hồi object vẫn còn được giữ sống.

**Tự làm:** Vẽ list chứa mọi request đã xử lý mà không xóa. **Gợi ý:** Hỏi ai giữ list và lúc nào dữ liệu được bỏ.

## App Leak Có Chủ Đích

Retention là giữ reference tới object; ví dụ nhỏ cho thấy list giữ các mảng cho tới khi bỏ chúng.

```java
var retained = new java.util.ArrayList<byte[]>();
for (int i = 0; i < 3; i++) retained.add(new byte[1024]); // Giữ ba mảng
System.out.println(retained.size()); // 3
retained.clear(); // Bỏ reference; không ép GC chạy ngay
```

**Khi nào dùng**

- Khi cần thao tác này trong bài thực hành.
- Khi đọc code có cùng cách viết.

**Lỗi hay gặp**

- Dùng trước khi hiểu kiểu đầu vào và kết quả.

**Tự gõ, không dùng AI sinh code:** Thêm giới hạn số phần tử giữ lại. **Gợi ý:** Dùng kích thước nhỏ; không cần làm máy hết bộ nhớ để học.

## Khi Phân Tích Heap Dump

Heap dump là ảnh chụp object trong heap để xem dữ liệu nào giữ dữ liệu nào.

**Quy tắc ngón tay cái:** Tìm chuỗi giữ reference, không chỉ object lớn nhất.

**Tự làm:** Vẽ đường từ một list còn sống tới phần tử hết cần. **Gợi ý:** Một object lớn có thể hoàn toàn hợp lệ; cần đối chiếu vòng đời nghiệp vụ.

## Bài Tập Nhanh

Tìm hiểu retention trước khi tạo dump.

- Tạo ví dụ giữ ba mảng.
- Xác định nơi bỏ reference.
- Ghi cách kiểm tra nếu list tăng mãi.

**Gợi ý:** Bài tạo dump đầy đủ chỉ làm trên app lab riêng.

**Đạt khi:** Giải thích ai giữ dữ liệu và thời điểm dữ liệu nên được giải phóng. Tự gõ phần triển khai, không dùng AI sinh lời giải.

## Đọc thêm sau

Lệnh tạo dump: `jcmd <pid> GC.heap_dump /tmp/task-lab.hprof` với PID ứng dụng bài tập còn chạy. Dump có thể chứa dữ liệu của app. Retained size (dung lượng được giữ sống), GC roots (điểm bắt đầu tìm object sống) và dominator tree (cây giữ sống) học khi phân tích dump.
