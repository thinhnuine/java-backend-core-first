# List, Set Và Map Internals

**Tài liệu tra cứu, không bắt buộc ở lượt đầu.** Đọc [bài 00](00-collections-first.md) là đủ để làm bài cơ bản. Tên file giữ để liên kết cũ vẫn dùng được.

Nếu muốn xem thêm, chỉ chọn đúng class đang gặp; các block chạy độc lập bằng JShell 21 như bài 00. Var cho Java suy ra kiểu biến; equals là phép kiểm tra bằng nhau, hashCode tính số hỗ trợ tra cứu.

## `ArrayList`

ArrayList là danh sách có thể thêm/xóa và đọc bằng vị trí bắt đầu từ 0.

```java
var names = new java.util.ArrayList<String>();
names.add("Java"); // Thêm cuối danh sách
names.add("SQL");
System.out.println(names.get(0)); // Java
names.remove("SQL"); // Xóa theo giá trị
```

**Khi nào dùng**

- Danh sách cần giữ thứ tự.
- Lựa chọn ban đầu cho List thông thường.

**Lỗi hay gặp**

- Đọc vị trí bằng size(), vượt giới hạn.
- Nghĩ contains luôn tra nhanh như Map.

**Quy tắc ngón tay cái:** Chưa có nhu cầu đặc biệt: bắt đầu với ArrayList.

**Tự gõ, không dùng AI sinh code:** Thêm ba tên rồi đọc phần tử cuối. **Gợi ý:** Vị trí cuối là size() - 1.

## `LinkedList`

**Để sau.**

LinkedList là danh sách nối các phần tử với nhau; nó cũng hỗ trợ lấy/thêm ở hai đầu.

```java
var queue = new java.util.LinkedList<String>();
queue.addLast("first"); // Đưa vào cuối
queue.addLast("second");
System.out.println(queue.removeFirst()); // first
```

**Khi nào dùng**

- Khi cần đọc code đang dùng LinkedList.
- Khi muốn thử thao tác hai đầu danh sách.

**Lỗi hay gặp**

- Nghĩ chèn giữa luôn nhanh dù phải tìm vị trí.
- Dùng get(i) lặp trên danh sách lớn.

**Quy tắc ngón tay cái:** Đừng chọn LinkedList chỉ vì nghe nói thêm/xóa nhanh.

**Tự gõ, không dùng AI sinh code:** Thêm phần tử thứ ba và lấy lần lượt cho đến hết. **Gợi ý:** Kiểm tra isEmpty trước removeFirst.

## `HashSet`

HashSet lưu các phần tử không trùng theo equals, hỗ trợ kiểm tra có phần tử hay không.

```java
var emails = new java.util.HashSet<String>();
emails.add("a@example.com");
emails.add("a@example.com"); // Trùng nên không tăng kích thước
System.out.println(emails.size()); // 1
System.out.println(emails.contains("a@example.com")); // true
```

**Khi nào dùng**

- Loại trùng dữ liệu.
- Kiểm tra thành viên.

**Lỗi hay gặp**

- Trông chờ thứ tự in cố định.
- Dùng object tự viết có equals/hashCode không nhất quán.

**Quy tắc ngón tay cái:** Cần không trùng, không cần thứ tự: HashSet.

**Tự gõ, không dùng AI sinh code:** Loại trùng ba email do bạn tự đặt. **Gợi ý:** Kiểm tra size trước và sau thêm trùng.

## `HashMap`

HashMap ghép key (khóa) với value (giá trị), giúp tra cứu theo id thay vì duyệt cả danh sách.

```java
var titles = new java.util.HashMap<Long, String>();
titles.put(1L, "Java"); // Lưu theo khóa 1
titles.put(1L, "SQL"); // Cùng khóa: thay giá trị
System.out.println(titles.get(1L)); // SQL
System.out.println(titles.containsKey(2L)); // false
```

**Khi nào dùng**

- Tra task theo id.
- Đếm số lần xuất hiện của một giá trị.

**Lỗi hay gặp**

- Nhầm containsValue với tra khóa.
- Sửa field tham gia hashCode của key sau khi đưa vào map.

**Quy tắc ngón tay cái:** Tra theo id: id là key, dữ liệu là value.

**Tự gõ, không dùng AI sinh code:** Lưu hai task theo id rồi cập nhật một id. **Gợi ý:** Quan sát size khi put lại khóa cũ.

## `LinkedHashMap`

**Chỉ đọc khi cần giữ thứ tự thêm key.**

LinkedHashMap là map có thứ tự duyệt xác định; mặc định giữ thứ tự thêm khóa.

```java
var titles = new java.util.LinkedHashMap<Long, String>();
titles.put(2L, "SQL");
titles.put(1L, "Java");
System.out.println(titles.keySet()); // [2, 1]: thứ tự thêm
```

**Khi nào dùng**

- Cần tra theo khóa và giữ thứ tự thêm.
- Tạo báo cáo có thứ tự ổn định.

**Lỗi hay gặp**

- Nghĩ thứ tự thêm và thứ tự truy cập tự hoạt động cùng lúc.

**Quy tắc ngón tay cái:** Thứ tự truy cập cần cấu hình riêng; mặc định là thứ tự thêm.

**Tự gõ, không dùng AI sinh code:** Gọi get(2L), rồi in keySet lần nữa. **Gợi ý:** Bản mặc định không đổi thứ tự khi đọc.

## Bài Tập Nhanh

Tự chọn; không phải điều kiện để đi tiếp.

Chọn loại chứa trước khi viết nhiều code.

- Lưu danh sách tên theo thứ tự.
- Loại tên trùng.
- Tra tên theo id.

**Gợi ý:** Viết cùng dữ liệu bằng List, Set, Map rồi chỉ ra thao tác nào tiện hơn.

**Đạt khi:** Tự giải thích mỗi lựa chọn bằng một thao tác cụ thể. Tự gõ phần triển khai, không dùng AI sinh lời giải.

## Đọc thêm sau

- Equals và hashCode phải đi cùng nhau khi tự viết key; bằng nhau phải cùng hash, ngược lại chưa chắc.
- Key mutable (đổi được trạng thái) có thể mất lookup (tra cứu) nếu đổi field dùng trong equality (tiêu chí bằng nhau).
- LRU cache loại mục ít truy cập gần đây nhất; access order (thứ tự truy cập) chưa tự làm cache có giới hạn, cần chính sách loại bỏ.
- Initial capacity là sức chứa ban đầu; resize là đổi sức chứa; ListIterator là công cụ duyệt list, thêm/xóa tại vị trí đã đứng không cần tìm lại; API không lộ node nội bộ.
- Cache locality mô tả ảnh hưởng vị trí dữ liệu trong bộ nhớ; collision là trùng hash, load factor là ngưỡng mật độ lưu; phân tích trường hợp xấu để sau.

ArrayList: get O(1), thêm cuối trung bình khấu hao O(1), chèn/xóa giữa O(n). HashMap: tra khóa trung bình O(1) khi hash phân bố tốt; containsValue O(n). O(n) nghĩa là công việc tăng theo số phần tử. Cấu trúc mảng, node, bucket và resize học khi cần phân tích hiệu năng.
