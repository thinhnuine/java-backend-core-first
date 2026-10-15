# Collections: Danh Sách, Tập Hợp Và Tra Theo ID

Bài làm quen: chỉ cần biết dùng trước. JDK 21 là bộ công cụ Java; mở terminal, gõ `jshell`, nhập code dưới đây. JShell chạy lệnh Java trực tiếp, không cần main; `/reset` để làm lại, `/exit` để thoát.

## 1. Ý chính

`List` là danh sách có thứ tự, cho phép trùng. `Set` là tập phần tử không trùng. `Map` ghép key (khóa, ví dụ ID) với value (giá trị, ví dụ title).

ArrayList, HashSet, HashMap là các class cụ thể dùng để tạo ba loại trên. `import` giúp viết tên class ngắn; `new` tạo object; `<String>` chỉ kiểu dữ liệu được chứa.

## 2. Giải thích code

Tự gõ từng nhóm, dừng ở mỗi dòng in kết quả:

```java
import java.util.*;
List<String> titles = new ArrayList<>();
titles.add("Java");
titles.add("Java"); // List cho phép trùng
System.out.println(titles.get(0)); // Java: vị trí đầu là 0
System.out.println(titles.size()); // 2: số phần tử
Set<String> unique = new HashSet<>(titles); // Lấy phần tử không trùng
System.out.println(unique.size()); // 1
Map<String, String> byId = new HashMap<>();
byId.put("t1", "Java");
byId.put("t1", "SQL"); // Cùng key: thay value cũ
System.out.println(byId.get("t1")); // SQL
System.out.println(byId.get("t9")); // null: không có key
byId.remove("t1"); // Xóa theo key
System.out.println(byId.containsKey("t1")); // false
```

`add` thêm phần tử; `get` đọc; `put` lưu cặp key/value; `containsKey` kiểm tra có key. `null` biểu thị không có giá trị ở kết quả đọc này.

## 3. Vì sao thiết kế như vậy

Hai task có thể cùng title. Dùng Set để lưu title sẽ bỏ trùng; dùng title làm key sẽ ghi đè. Muốn giữ từng task thì đặt ID riêng như t1, t2.

## 4. Liên hệ với frontend

List gần Array; Set/Map gần các loại cùng tên trong JS. Chưa cần liên hệ cách chúng hoạt động bên trong.

## 5. Khi nào dùng và không dùng

| Bạn muốn | Chọn |
| --- | --- |
| Giữ danh sách, có thể trùng | List |
| Loại trùng hoặc kiểm có phần tử không | Set |
| Tìm dữ liệu theo ID | Map |

Đừng dùng vị trí trong List làm ID cố định: xóa phần tử sẽ đổi vị trí phía sau.

## 6. Bẫy hay gặp

- `Map.get` trả null khi không có key. HashMap cho phép value null, nên dùng containsKey nếu cần phân biệt hai trường hợp.
- Put lại cùng key sẽ thay value, không tạo thêm entry (một cặp key/value).
- HashMap/HashSet không bảo đảm thứ tự in ra.
- List bắt đầu từ 0; với ba phần tử, vị trí cuối là 2.

## 7. Thuật ngữ mới

Collection là tên gọi chung cho bộ chứa phần tử; interface Collection là một kiểu trong Java, còn Map không kế thừa nó. Interface là hợp đồng thao tác; implementation là class thực hiện hợp đồng. Chỉ cần nhận biết các từ này khi đọc.

## 8. Bài tập nhỏ để tự gõ lại

1. Lưu ba title, hai title trùng; đoán size List/Set rồi chạy.
2. Lưu ba title bằng ba ID; đọc một ID có thật và một ID không có.
3. Thay title ở một ID, xóa một ID, kiểm tra lại.

Đủ khi tự làm và giải thích output. Sau đó làm [bài tập cơ bản](03-exercises.md); chưa cần học bài 02.

## Đọc thêm sau

- Ví dụ thừa và output dài đã gộp vào demo; Task/save/find/delete để sau khi quen class.
- Boxing chuyển primitive thành object: 1 thành Integer, 1L thành Long; Map có key Long sẽ không tìm thấy bằng 1 vì sai kiểu, dù get vẫn biên dịch.
- Equality là tiêu chí bằng nhau; equals kiểm tra nó, hashCode tính số hỗ trợ tra cứu; khi tự viết key, equals và hashCode phải đi cùng nhau.
- Key mutable (đổi được trạng thái) có thể mất lookup (tra cứu) nếu đổi field dùng trong equality.
- Không trả list nội bộ cho nơi khác sửa tùy ý; không chia sẻ HashMap cho nhiều thread (luồng thực thi) ghi thiếu bảo vệ.
- containsValue tìm theo value, không có cùng đặc tính tra key của get; hiệu năng để sau.
