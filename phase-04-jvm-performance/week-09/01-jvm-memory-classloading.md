# JVM Memory Và Class Loading

> **Làm rõ trước khi học:** String pool không phải một vùng bộ nhớ ngang hàng với heap/stack. Không thể kết luận nơi cấp phát thực tế chỉ từ việc thấy new; JVM có tối ưu.

**Cách chạy ví dụ:** dùng JDK 21 (bộ công cụ Java). Mở terminal, gõ `jshell`; gõ từng khối Java độc lập, dùng `/reset` trước khối tiếp theo và `/exit` để thoát. JShell chạy được khai báo class, method (hàm của Java) và lệnh trực tiếp, không cần khung main. Các comment `//` giải thích dòng code và kết quả cần quan sát.

## Các Vùng Memory Chính

JVM là máy thực thi Java; heap chứa dữ liệu object theo mô hình học, stack theo dõi các lời gọi method, metaspace giữ thông tin mô tả class.

**Quy tắc ngón tay cái:** Học vai trò trước khi học bố trí byte trong bộ nhớ.

**Tự làm:** Vẽ object Task và method đang dùng nó. **Gợi ý:** Biến reference và object được tham chiếu là hai thứ khác nhau.

## Heap

Heap là vùng bộ nhớ Java quản lý cho object; object còn được tham chiếu tới có thể vẫn cần được giữ.

```java
var tasks = new java.util.ArrayList<String>(); // Giữ các chuỗi trong danh sách
tasks.add("Java");
tasks.clear(); // Bỏ phần tử khỏi list, không phải lệnh bắt GC chạy
System.out.println(tasks.size()); // 0
```

**Khi nào dùng**

- Khi cần thao tác này trong bài thực hành.
- Khi đọc code có cùng cách viết.

**Lỗi hay gặp**

- Dùng trước khi hiểu kiểu đầu vào và kết quả.

**Tự gõ, không dùng AI sinh code:** Thêm rồi xóa một phần tử và giải thích reference nào được bỏ. **Gợi ý:** Không suy ra thời điểm bộ nhớ được thu hồi từ size().

## Stack

Stack theo dõi các lời gọi method đang thực hiện; gọi lồng nhau tạo thêm bước phải quay về.

```java
int plusOne(int value) { return value + 1; }
int plusTwo(int value) { return plusOne(plusOne(value)); }
System.out.println(plusTwo(3)); // 5
```

**Khi nào dùng**

- Khi cần thao tác này trong bài thực hành.
- Khi đọc code có cùng cách viết.

**Lỗi hay gặp**

- Dùng trước khi hiểu kiểu đầu vào và kết quả.

**Tự gõ, không dùng AI sinh code:** Vẽ thứ tự các lời gọi trong plusTwo. **Gợi ý:** Đọc lời gọi trong cùng trước; không tạo đệ quy vô hạn.

## Metaspace

Metaspace là bộ nhớ HotSpot (một triển khai JVM phổ biến) dùng cho thông tin mô tả class.

**Quy tắc ngón tay cái:** Lượt đầu biết nó khác dữ liệu object là đủ.

**Tự làm:** Phân biệt “class Task có field gì” với “task này có title gì”. **Gợi ý:** Thông tin về kiểu và giá trị từng object có vai trò khác nhau.

## Class Loading

Class loading là quá trình đưa định nghĩa class vào môi trường chạy để Java có thể sử dụng.

```java
Class<?> type = Class.forName("java.lang.String"); // Tìm class theo tên đầy đủ
System.out.println(type.getSimpleName()); // String
```

**Khi nào dùng**

- Khi cần thao tác này trong bài thực hành.
- Khi đọc code có cùng cách viết.

**Lỗi hay gặp**

- Dùng trước khi hiểu kiểu đầu vào và kết quả.

**Tự gõ, không dùng AI sinh code:** Thử một tên class không tồn tại và đọc lỗi. **Gợi ý:** Tên đầy đủ gồm package; khôi phục tên đúng sau khi thử.

## Class Initialization

Class initialization là chạy phần khởi tạo tĩnh của class khi nó được khởi tạo lần đầu.

```java
class Config { static int limit = 10; } // static: dữ liệu thuộc class
System.out.println(Config.limit); // 10
```

**Khi nào dùng**

- Khi cần thao tác này trong bài thực hành.
- Khi đọc code có cùng cách viết.

**Lỗi hay gặp**

- Dùng trước khi hiểu kiểu đầu vào và kết quả.

**Tự gõ, không dùng AI sinh code:** Thêm static block in một thông báo rồi đọc limit hai lần. **Gợi ý:** Quan sát block chạy mấy lần trong một lượt nạp class.

## Bài Tập Nhanh

Lập sơ đồ bộ nhớ ở mức ý nghĩa.

- Tạo một Task và hai biến tham chiếu cùng task.
- Gọi method đọc title.
- Vẽ lời gọi và object.

**Gợi ý:** Không cần đo địa chỉ hay kích thước byte.

**Đạt khi:** Không nhầm biến reference với bản sao object. Tự gõ phần triển khai, không dùng AI sinh lời giải.

## Đọc thêm sau

Native memory (bộ nhớ ngoài heap), classloader (bộ nạp class), escape analysis (phân tích khả năng object thoát ra ngoài) và lỗi hết metaspace học khi chẩn đoán thực tế.
