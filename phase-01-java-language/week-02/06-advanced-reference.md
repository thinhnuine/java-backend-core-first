# Tra Cứu Sau: Generics Và Class Lồng Nhau

> **Làm rõ trước khi học:** `List<?>` không phải list chứa tùy ý mọi kiểu. `List<? extends Number>` không có nghĩa được thêm bất kỳ Number nào. Type erasure không xóa mọi thông tin generic khỏi mọi metadata.

Đây là bản tra cứu tự chọn. Các tiêu đề cũ được giữ để bạn tìm lại; không cần học hết trước Spring.

**Cách chạy ví dụ:** dùng JDK 21 (bộ công cụ Java). Mở terminal, gõ `jshell`; gõ từng khối Java độc lập, dùng `/reset` trước khối tiếp theo và `/exit` để thoát. JShell chạy được khai báo class, method (hàm của Java) và lệnh trực tiếp, không cần khung main. Các comment `//` giải thích dòng code và kết quả cần quan sát.

## 2. Generic Method

Generic method là method tự khai báo kiểu đại diện T để giữ liên hệ kiểu đầu vào và đầu ra.

```java
<T> T first(java.util.List<T> values) { return values.get(0); }
System.out.println(first(java.util.List.of("Java"))); // Java: trả String
```

**Khi nào dùng**

- Một helper dùng nhiều kiểu dữ liệu.
- Đầu vào và đầu ra cần cùng kiểu.

**Lỗi hay gặp**

- Quên xử lý danh sách rỗng trước get(0).

**Quy tắc ngón tay cái:** T xuất hiện ở nhiều vị trí khi các vị trí đó phải cùng kiểu.

**Tự gõ, không dùng AI sinh code:** Gọi first với danh sách số rồi danh sách rỗng. **Gợi ý:** Trường hợp rỗng cần tự chọn cách xử lý.

## 3. Bounded Type

Bounded type giới hạn kiểu được chọn cho T, để method biết được phép gọi hành vi nào.

```java
<T extends Number> double value(T number) { // T phải là Number hoặc kiểu con
    return number.doubleValue(); // Lấy dạng double
}
System.out.println(value(3)); // 3.0
```

**Khi nào dùng**

- Method cần hành vi của một nhóm kiểu.
- Muốn compiler chặn kiểu không phù hợp.

**Lỗi hay gặp**

- Nghĩ extends ở generic chỉ áp dụng class, không áp dụng interface.

**Quy tắc ngón tay cái:** Cần hành vi của Number thì giới hạn T theo Number.

**Tự gõ, không dùng AI sinh code:** Gọi value với 3.5 rồi thử một String. **Gợi ý:** String không thuộc nhóm Number.

## 4. Wildcard

Wildcard là dấu ? biểu thị một kiểu chưa biết, hữu ích khi chỉ cần đọc object từ danh sách.

```java
java.util.List<?> values = java.util.List.of("Java", "SQL");
Object first = values.get(0); // Chỉ bảo đảm đọc được dạng Object
System.out.println(first); // Java
```

**Khi nào dùng**

- Chỉ cần đọc object từ danh sách.
- Không cần giữ tên kiểu cụ thể ở đầu ra.

**Lỗi hay gặp**

- Nghĩ ? có nghĩa có thể thêm mọi kiểu.

**Quy tắc ngón tay cái:** Dấu ? là kiểu chưa biết, không phải mọi kiểu đều được ghi vào.

**Tự gõ, không dùng AI sinh code:** Thử thêm một String vào biến values và đọc lỗi kiểu. **Gợi ý:** Không biết kiểu thật thì không được tự chọn kiểu để thêm.

## 5. PECS

PECS là mẹo chọn wildcard: extends cho nguồn đọc theo một kiểu, super cho nơi nhận giá trị kiểu đó.

```java
java.util.List<? extends Number> source = java.util.List.of(1, 2);
Number value = source.get(0); // Đọc Number an toàn
java.util.List<? super Integer> target = new java.util.ArrayList<Number>();
target.add(3); // Có thể ghi Integer
System.out.println(target); // [3]
```

**Khi nào dùng**

- Nhận danh sách chỉ để đọc số.
- Nhận nơi chứa để thêm giá trị.

**Lỗi hay gặp**

- Cố thêm số vào ? extends Number.
- Đọc ? super Integer và giả định kết quả luôn Integer.

**Quy tắc ngón tay cái:** Đọc T: extends T; ghi T: super T.

**Tự gõ, không dùng AI sinh code:** Thử đọc target vào Integer rồi giải thích lỗi. **Gợi ý:** Kết quả đọc chỉ được bảo đảm là Object.

## 6. Type Erasure

Type erasure là cách Java bỏ phần lớn tham số kiểu khỏi dạng thực thi, nên không kiểm tra `List<String>` như một class riêng khi chạy.

```java
var words = new java.util.ArrayList<String>(); // var suy ra kiểu biến
var counts = new java.util.ArrayList<Integer>();
System.out.println(words.getClass() == counts.getClass()); // true
```

**Khi nào dùng**

- Đọc giới hạn của generic khi chạy.
- Thiết kế code cần biết kiểu thực tế.

**Lỗi hay gặp**

- Kiểm instanceof `List<String>`.
- Tưởng mọi thông tin generic đều bị xóa hoàn toàn.

**Quy tắc ngón tay cái:** Kiểm tra kiểu phần tử chủ yếu diễn ra khi biên dịch.

**Tự gõ, không dùng AI sinh code:** Tạo `ArrayList<Long>` và so getClass với words. **Gợi ý:** getClass trả class thực tế, không trả tham số generic.

## 2. Enum Có Field Và Behavior

Enum có thể giữ dữ liệu kèm theo mỗi lựa chọn, chẳng hạn nhãn hiển thị.

```java
enum Status {
    TODO("Cần làm"), DONE("Xong");
    private final String label;
    Status(String label) { this.label = label; } // Khởi tạo nhãn
    String label() { return label; }
}
System.out.println(Status.TODO.label()); // Cần làm
```

**Khi nào dùng**

- Mỗi trạng thái cần một nhãn.
- Thông tin đi kèm lựa chọn ổn định.

**Lỗi hay gặp**

- Dùng enum cho danh sách do người dùng thêm tùy ý.

**Tự gõ, không dùng AI sinh code:** Thêm IN_PROGRESS có nhãn riêng. **Gợi ý:** Mỗi hằng phải truyền nhãn cho constructor.

## 3. Enum Cho State Transition

State transition là quy tắc cho phép đổi từ trạng thái hiện tại sang trạng thái khác.

```java
enum Step { TODO, DONE }
boolean canMove(Step from, Step to) { return from == Step.TODO && to == Step.DONE; }
System.out.println(canMove(Step.DONE, Step.TODO)); // false theo quy tắc mẫu
```

**Khi nào dùng**

- Nghiệp vụ có bước trước/sau rõ.
- Cần từ chối chuyển trạng thái trái phép.

**Lỗi hay gặp**

- Nghĩ enum tự áp đặt thứ tự chuyển.

**Quy tắc ngón tay cái:** Enum liệt kê trạng thái; quy tắc chuyển là phần bạn viết thêm.

**Tự gõ, không dùng AI sinh code:** Viết quy tắc cho thêm trạng thái CANCELLED. **Gợi ý:** Vẽ các mũi tên được phép trước khi viết if.

## 4. Static Nested Class

Static nested class là class đặt trong class khác nhưng không cần một object của class ngoài để tạo.

```java
class Page { static class Size { int value() { return 20; } } }
Page.Size size = new Page.Size(); // Không cần new Page
System.out.println(size.value()); // 20
```

**Khi nào dùng**

- Một kiểu chỉ có ý nghĩa gần class ngoài.
- Muốn gom tên mà không giữ object ngoài.

**Lỗi hay gặp**

- Bỏ static dù không cần dữ liệu của object ngoài.

**Quy tắc ngón tay cái:** Chỉ gom tên và code: ưu tiên static nested class.

**Tự gõ, không dùng AI sinh code:** Tạo cấu hình kích thước khác trong Size. **Gợi ý:** Dùng constructor nhận số.

## 5. Inner Class

Inner class là class lồng không có static, gắn với một object của class ngoài.

```java
class Page {
    int size = 20;
    class Cursor { int limit() { return size; } } // Đọc field của Page
}
Page page = new Page();
System.out.println(page.new Cursor().limit()); // 20
```

**Khi nào dùng**

- Class con cần đọc state của object ngoài.
- Một object phụ gắn với một instance cụ thể.

**Lỗi hay gặp**

- Không nhận ra object trong giữ liên hệ với object ngoài.

**Quy tắc ngón tay cái:** Cần dữ liệu của object ngoài mới cân nhắc inner class.

**Tự gõ, không dùng AI sinh code:** Tạo hai Page có size khác nhau và hai Cursor tương ứng. **Gợi ý:** Mỗi Cursor gắn với Page đã tạo nó.

## 6. Anonymous Class

Anonymous class là class không đặt tên, được khai báo ngay lúc tạo object để dùng một lần.

```java
Runnable job = new Runnable() { // Runnable cam kết có method run
    public void run() { System.out.println("Hello"); }
};
job.run(); // Hello, chưa tạo thread
```

**Khi nào dùng**

- Một implementation nhỏ chỉ dùng một nơi.
- Đọc code Java cũ chưa dùng lambda.

**Lỗi hay gặp**

- Nhét quá nhiều xử lý vào class không tên.
- Nhầm gọi run với tạo thread mới.

**Tự gõ, không dùng AI sinh code:** Đổi nội dung run rồi gọi hai lần. **Gợi ý:** run trực tiếp chạy trên luồng gọi hiện tại.

## Lưu ý khi đọc

Chỉ đọc một mục khi gặp nhu cầu tương ứng. Bài Box, enum và List cơ bản đủ để tiếp tục Task Manager.

## Đọc thêm sau

Đọc về capture (compiler đặt tên tạm cho kiểu chưa biết), bridge method (method hỗ trợ sau khi xóa kiểu) và giới hạn nhiều kiểu khi đã đọc được PECS. Nguồn: [Java Language Specification 21, types](https://docs.oracle.com/javase/specs/jls/se21/html/jls-4.html).
