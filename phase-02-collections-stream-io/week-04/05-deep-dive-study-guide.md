# Mentor Guide Tuần 4: Ôn Tập Nhẹ

Dù tên file còn là deep-dive, lượt này chỉ ôn một việc: đọc Map. Nếu đã hiểu bài 00 thì bỏ qua. Chạy code trong JShell 21.

## 1. Ý chính

Map là bộ chứa ghép key (khóa) với value (giá trị), giúp tìm dữ liệu bằng ID.

## 2. Giải thích code

```java
var titles = new java.util.HashMap<String, String>();
titles.put("t1", "Java"); // Lưu title theo ID
System.out.println(titles.get("t1")); // Java
System.out.println(titles.get("t2")); // null: thiếu ID
System.out.println(titles.containsKey("t2")); // false
```

Var cho Java suy ra kiểu biến từ giá trị bên phải, không biến nó thành any. Chưa cần học thêm API trong ví dụ này.

## 3. Vì sao thiết kế như vậy

ID t2 chưa được lưu nên không có title để trả về. Không thêm dữ liệu giả chỉ để tránh null; hãy kiểm tra và xử lý trường hợp thiếu kết quả.

## 4. Liên hệ với Frontend

JS Map.get trả undefined khi thiếu key; Java Map.get trả null. Đừng viết dựa trên giả định luôn tìm thấy dữ liệu.

## 5. Khi nào dùng và không dùng

Tra theo ID dùng Map; cần danh sách dùng List; cần loại trùng dùng Set. Lượt đầu dùng String làm ID và value để dễ quan sát.

## 6. Bẫy hay gặp

Get thiếu key không tự tạo dữ liệu. HashMap cũng cho lưu value null; containsKey giúp phân biệt. Đừng học thêm nhiều API khi chưa giải thích được ba dòng output trên.

## 7. Thuật ngữ mới

Lookup là tra cứu. Null là không có giá trị; khác String rỗng `""`.

## 8. Bài tập nhỏ để tự gõ lại

Lưu hai title theo hai ID; đọc một ID không có. Thêm ID đó rồi đọc lại; giải thích điều gì thay đổi. Chưa cần class Task.

Đủ khi giải thích được output; sau đó tiếp tục bài Java cơ bản trong roadmap.

## Đọc thêm sau

- Demo lookup 100k số, key UserId và blocked ID trong loop chuyển về phần tự chọn bài 01–03; dùng HashSet tránh quét List lặp.
- JS Array/Map/Set và lựa chọn type: bài 00 và generics; Big-O/hash/bucket: bài 01–02.
- Equals và hashCode phải đi cùng nhau; key mutable có thể mất lookup; ConcurrentHashMap không bảo vệ mọi flow nhiều bước.
- Final ngăn gán lại reference (tham chiếu), không làm object immutable (trạng thái không đổi); gần với JS const.
- Record là class dữ liệu gọn, tự sinh equals/hashCode; chỉ bất biến nông: object bên trong vẫn có thể mutable.
- View dùng chung dữ liệu nguồn; copy có cấu trúc chứa riêng nhưng có thể chung object phần tử; unmodifiable chặn sửa qua API đó, không khóa nguồn.
- Defensive copy là bản copy bảo vệ cấu trúc nội bộ khỏi thay đổi qua reference ngoài; không tự làm mọi phần tử immutable.

Ví dụ tự chọn cho các ý final/record/view/copy; **chưa cần chạy ở lượt đầu**, `/reset` trước khi thử:

```java
import java.util.*;
record Group(List<String> names) {}
final List<String> source = new ArrayList<>();
source.add("An"); // final vẫn cho sửa nội dung
var group = new Group(source);
var view = Collections.unmodifiableList(source); // Chỉ chặn sửa qua view
var copy = List.copyOf(source); // Tách cấu trúc, không cho sửa
group.names().add("Binh"); // record vẫn chứa list mutable
System.out.println(view); // [An, Binh]: thấy nguồn đổi
System.out.println(copy); // [An]: cấu trúc copy không đổi
```

- Chọn view khi muốn chặn sửa qua API nhưng vẫn thấy nguồn đổi; chọn copy để tách cấu trúc khỏi nguồn.
- Bài tự gõ sau này: tạo list/view/copy, sửa nguồn và đoán output; thử list nằm trong record. Không kèm đáp án.
