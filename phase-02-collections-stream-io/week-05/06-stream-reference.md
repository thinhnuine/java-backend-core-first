# Tra Cứu Stream Và Collector

> **Làm rõ trước khi học:** Stream.toList không cho sửa cấu trúc kết quả; Collectors.toList không cam kết implementation cụ thể. Optional không tự ngăn bạn gọi get sai.

**Cách chạy ví dụ:** dùng JDK 21 (bộ công cụ Java). Mở terminal, gõ `jshell`; gõ từng khối Java độc lập, dùng `/reset` trước khối tiếp theo và `/exit` để thoát. JShell chạy được khai báo class, method (hàm của Java) và lệnh trực tiếp, không cần khung main. Các comment `//` giải thích dòng code và kết quả cần quan sát.

## Stream API

Stream API là các method ghép thành lượt xử lý dữ liệu.

```java
var names = java.util.List.of("An", "Thinh");
var longNames = names.stream().filter(name -> name.length() > 2).toList();
System.out.println(longNames); // [Thinh]
```

**Khi nào dùng**

- Lọc danh sách theo điều kiện.
- Biến đổi dữ liệu mà không sửa nguồn.

**Lỗi hay gặp**

- Quên thao tác kết thúc như toList.

**Tự gõ, không dùng AI sinh code:** Đổi điều kiện để không giữ tên nào. **Gợi ý:** Kết quả danh sách rỗng là hợp lệ.

## Operation Hay Dùng

Operation là một bước xử lý; distinct bỏ trùng, sorted sắp xếp, limit lấy tối đa một số phần tử.

```java
var values = java.util.List.of(3, 1, 3, 2);
System.out.println(values.stream().distinct().sorted().limit(2).toList()); // [1, 2]
```

**Khi nào dùng**

- Cần chọn, bỏ trùng và sắp xếp.
- Giới hạn số kết quả.

**Lỗi hay gặp**

- Đổi thứ tự bước rồi mong kết quả luôn giống nhau.

**Tự gõ, không dùng AI sinh code:** Đổi thứ tự limit và sorted rồi so kết quả. **Gợi ý:** Dự đoán trước khi chạy vì thứ tự bước có ý nghĩa.

## Collector

Collector mô tả cách gom phần tử, chẳng hạn nhóm từ theo độ dài.

```java
var words = java.util.List.of("An", "Ha", "Thinh");
var groups = words.stream().collect(java.util.stream.Collectors.groupingBy(String::length));
System.out.println(groups.get(2)); // [An, Ha]; :: gọi method length cho mỗi chuỗi
```

**Khi nào dùng**

- Nhóm phần tử theo thuộc tính.
- Tính thống kê theo nhóm.

**Lỗi hay gặp**

- Nhầm kiểu key với kiểu dữ liệu gốc.
- Mong thứ tự HashMap được bảo đảm.

**Tự gõ, không dùng AI sinh code:** Nhóm các từ bạn tự chọn và lấy nhóm dài 3. **Gợi ý:** Kiểu khóa của map là Integer vì length trả số.

## Optional

Optional là hộp kết quả có thể có hoặc không có giá trị, giúp bên gọi nhớ xử lý trường hợp thiếu.

```java
java.util.Optional<String> title = java.util.Optional.empty(); // Không có giá trị
System.out.println(title.orElse("Không có")); // Chọn giá trị dự phòng
var present = java.util.Optional.of("Java"); // Có giá trị khác null
System.out.println(present.orElse("Không có")); // Java
```

**Khi nào dùng**

- Trả kết quả tìm kiếm có thể thiếu.
- Biểu thị vắng mặt thay cho giá trị giả.

**Lỗi hay gặp**

- Gọi get trên Optional rỗng.
- Gọi of(null).

**Quy tắc ngón tay cái:** Optional có thể rỗng: chọn cách xử lý trước khi lấy giá trị.

**Tự gõ, không dùng AI sinh code:** Dùng ofNullable với biến String lúc có giá trị, lúc null. **Gợi ý:** In bằng orElse, chưa dùng get.

## Functional Interface

Functional interface là interface có một method abstract (method chưa có phần thân), nên có thể nhận một lambda.

```java
java.util.function.Predicate<String> longEnough = text -> text.length() > 3;
System.out.println(longEnough.test("Java")); // true: Predicate trả boolean
```

**Khi nào dùng**

- Truyền điều kiện kiểm tra vào method.
- Đưa hành vi ngắn từ ngoài vào.

**Lỗi hay gặp**

- Lambda trả sai kiểu mà method abstract yêu cầu.

**Tự gõ, không dùng AI sinh code:** Tạo Predicate kiểm tra chuỗi bắt đầu bằng J. **Gợi ý:** Dùng startsWith trong lambda.

## Lambda Và Method Reference

Lambda viết một hành vi ngắn; method reference dùng dấu :: để trỏ tới method đã có.

```java
var words = java.util.List.of("Java", "SQL");
System.out.println(words.stream().map(word -> word.length()).toList()); // [4, 3]
System.out.println(words.stream().map(String::length).toList()); // [4, 3]
```

**Khi nào dùng**

- Cung cấp hành vi cho map/filter.
- Dùng lại method đã có tên rõ.

**Lỗi hay gặp**

- Tưởng :: thực hiện lời gọi ngay tại dòng khai báo.

**Tự gõ, không dùng AI sinh code:** Viết hai cách lấy chữ thường từ danh sách. **Gợi ý:** Ưu tiên dạng bạn giải thích được; tên method thôi chưa phải lời gọi.

## Bài Tập Nhanh

Luyện pipeline (chuỗi bước) với dữ liệu nhỏ.

- Tạo 5 title có trùng.
- Bỏ trùng, chọn title dài hơn 3, sắp xếp.

**Gợi ý:** Viết loop trước nếu chưa rõ từng bước Stream.

**Đạt khi:** Hai cách cho cùng danh sách theo cùng thứ tự. Tự gõ phần triển khai, không dùng AI sinh lời giải.

## Đọc thêm sau

flatMap, groupingBy nhiều mức, orElseGet (tính giá trị dự phòng khi cần), và parallelStream để sau. [Stream Java 21](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/stream/Stream.html).
