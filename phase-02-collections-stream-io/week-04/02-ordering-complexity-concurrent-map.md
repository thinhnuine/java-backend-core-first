# Ordering, Complexity Và Concurrent Collections

**Bỏ qua toàn bộ file này ở lượt làm quen.** Đây là tài liệu để tra khi đã dùng List/Set/Map thành thạo; chưa cần nhớ Big-O hay viết nhiều thread.

Ví dụ chạy bằng JShell 21 như bài 00. Var cho Java suy ra kiểu biến; quy tắc so sánh quyết định thứ tự phần tử.

## `TreeMap`

TreeMap là map duyệt theo thứ tự khóa đã sắp xếp.

```java
var titles = new java.util.TreeMap<Integer, String>();
titles.put(2, "SQL"); titles.put(1, "Java");
System.out.println(titles.keySet()); // [1, 2]: theo khóa
```

**Khi nào dùng**

- Cần khóa luôn được sắp xếp.
- Tìm khóa gần một mốc.

**Lỗi hay gặp**

- Dùng trước khi hiểu kiểu đầu vào và kết quả.

**Quy tắc ngón tay cái:** Cần thứ tự khóa: TreeMap.

**Tự gõ, không dùng AI sinh code:** Thêm khóa 3 rồi 0. **Gợi ý:** Quan sát thứ tự khác LinkedHashMap.

## `TreeSet`

TreeSet loại trùng và sắp xếp theo quy tắc so sánh của phần tử.

```java
var scores = new java.util.TreeSet<Integer>();
scores.add(3); scores.add(1); scores.add(3);
System.out.println(scores); // [1, 3]
```

**Khi nào dùng**

- Khi cần thao tác này trong bài thực hành.
- Khi đọc code có cùng cách viết.

**Lỗi hay gặp**

- Quy tắc so sánh tùy chỉnh trả 0 cho hai phần tử bạn muốn giữ riêng.

**Tự gõ, không dùng AI sinh code:** Thêm nhiều số lặp và in số nhỏ nhất. **Gợi ý:** Dùng first sau khi bảo đảm set không rỗng.

## Big-O Cần Nhớ

Big-O mô tả lượng công việc tăng thế nào khi dữ liệu tăng, không phải số mili giây cố định. Lượt đầu chỉ cần: ArrayList.get gần như không phụ thuộc số phần tử; tìm một giá trị trong list phải duyệt; TreeMap tra khóa tăng chậm theo kích thước.

**Quy tắc ngón tay cái:** chọn đúng thao tác cần làm trước, đo sau.

**Tự làm:** so tra id bằng vòng lặp với Map. **Gợi ý:** đếm số phần tử phải xét, chưa cần bấm giờ.

## `ConcurrentHashMap`

Để sau khi học thread. Merge thêm số đếm khi thiếu key hoặc cộng vào số cũ; `Integer::sum` là hàm cộng hai Integer.

Để sau khi học thread. Merge thêm số đếm khi thiếu key hoặc cộng vào số cũ; `Integer::sum` là hàm cộng hai Integer.

ConcurrentHashMap là map hỗ trợ truy cập từ nhiều thread (luồng thực thi) cùng lúc; dùng thao tác gộp để cập nhật một khóa an toàn.

```java
var counts = new java.util.concurrent.ConcurrentHashMap<String, Integer>();
counts.merge("java", 1, Integer::sum); // Cộng 1 trong một thao tác theo khóa
counts.merge("java", 1, Integer::sum);
System.out.println(counts.get("java")); // 2
```

**Khi nào dùng**

- Có nhiều thread cùng dùng map.
- Cập nhật số đếm theo khóa.

**Lỗi hay gặp**

- Ghép get rồi put và nghĩ cả hai là một thao tác.
- Dùng null làm key/value.

**Quy tắc ngón tay cái:** Map an toàn không làm chuỗi nghiệp vụ nhiều bước tự an toàn.

**Tự gõ, không dùng AI sinh code:** Đếm hai từ có lặp bằng merge. **Gợi ý:** Chưa tạo thread; trước hết hiểu quy tắc cộng.

## Khi Nào Dùng Concurrent Collection

Concurrent collection là bộ chứa hỗ trợ nhiều luồng. Chỉ dùng khi dữ liệu thực sự được chia sẻ giữa các luồng; bài Java đầu tiên thường chưa cần. **Tự làm:** vẽ nơi tạo và nơi dùng map trong TaskService. **Gợi ý:** phân biệt biến cục bộ mỗi lời gọi với field dùng chung.

## Bài Tập Nhanh

Tự chọn sau này, không phải bài hoàn thành lượt đầu.

Làm một ví dụ thứ tự trước khi học đồng thời.

- Đưa cùng ba khóa vào HashMap, LinkedHashMap và TreeMap.
- Ghi cái nào bảo đảm thứ tự nào.

**Gợi ý:** Không suy ra bảo đảm từ một lần in của HashMap.

**Đạt khi:** Giải thích vì sao chọn map cho bài của mình. Tự gõ phần triển khai, không dùng AI sinh lời giải.

## Đọc thêm sau

- TreeSet xác định trùng khi so sánh trả 0; muốn tuân thủ hợp đồng Set, kết quả đó phải khớp equals; java/JAVA có thể bị coi trùng với so sánh không phân biệt hoa/thường.
- TreeMap sorted theo key, không theo value; range query là truy vấn theo khoảng.
- Big-O: O(1) không tăng theo n, O(n) tuyến tính, O(log n) tăng theo logarithm; amortized là bình quân trên chuỗi thao tác.
- ArrayList.get O(1), contains/xóa O(n), thêm cuối amortized O(1); HashSet/HashMap tra/thêm/xóa trung bình O(1), add/put amortized khi hash phân bố tốt.
- TreeSet/TreeMap tra/thêm/xóa O(log n); containsValue của map O(n), map không get theo index; giả sử hash/equals/so sánh O(1).
- Một lần đo không chứng minh Big-O; O(1) chưa chắc nhanh hơn trên input nhỏ; tree duyệt sorted, list sort riêng, hash không bảo đảm thứ tự.
- Merge thêm value khi thiếu key, hoặc tính value mới khi đã có; Integer::sum là hàm cộng hai Integer; computeIfAbsent tính value khi key chưa có.
- ConcurrentHashMap không tự bảo vệ value mutable hoặc nhiều key; chạy đúng nhiều lần không chứng minh thread safety.
- Ownership là ai quản lý state; lock phối hợp thread; invariant là điều kiện luôn đúng; transaction nhóm thao tác có bảo đảm chung, concurrent map không thay thế nó.
- Bài mở rộng: WordCounter input nhiều dòng, bản một thread dùng HashMap, nhiều thread dùng ConcurrentHashMap.merge; so tính đúng và độ dễ đọc.

Comparator (object cung cấp quy tắc so sánh), cân bằng cây, collision, atomic compound operation (cập nhật nhiều bước như một thao tác) học sau. [ConcurrentHashMap Java 21](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/concurrent/ConcurrentHashMap.html).
