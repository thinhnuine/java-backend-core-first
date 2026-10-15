# OOP Và Object Contract

> **Làm rõ trước khi học:** Object.equals mặc định so cùng object, không tự so field. Record không tự kiểm tra null và không tự đóng băng object bên trong. Interface có thể có method có phần thân; bài đầu chỉ dùng lời cam kết đơn giản.

**Cách chạy ví dụ:** dùng JDK 21 (bộ công cụ Java). Mở terminal, gõ `jshell`; gõ từng khối Java độc lập, dùng `/reset` trước khối tiếp theo và `/exit` để thoát. JShell chạy được khai báo class, method (hàm của Java) và lệnh trực tiếp, không cần khung main. Các comment `//` giải thích dòng code và kết quả cần quan sát.

## Interface

Interface là lời cam kết về các method mà object cung cấp, giúp bạn thay cách làm mà vẫn giữ cách gọi.

```java
interface Sender { void send(String text); } // Cam kết có send
class ConsoleSender implements Sender { // implements: thực hiện cam kết
    public void send(String text) { System.out.println(text); }
}
Sender sender = new ConsoleSender(); // Kiểu biến là khả năng cần dùng
sender.send("Hello"); // Hello: chạy cách làm của ConsoleSender
```

**Liên hệ FE:** Giống TS interface về mô tả khả năng; Java class phải khai báo implements hoặc thừa hưởng quan hệ đó, không chỉ có đúng tên method.

**Khi nào dùng**

- Khi cần hai cách gửi hoặc lưu dữ liệu.
- Khi muốn thay object thật bằng object đơn giản để kiểm thử.

**Lỗi hay gặp**

- Tạo interface cho mọi class dù chưa cần thay đổi.
- Quên public ở method thực hiện cam kết.

**Quy tắc ngón tay cái:** Cần thay cách làm: nghĩ đến interface.

**Tự gõ, không dùng AI sinh code:** Tạo UpperSender thực hiện Sender và in message chữ hoa. **Gợi ý:** Giữ lời gọi sender.send, chỉ thay object được tạo.

## Abstract Class

Abstract class là class chưa hoàn chỉnh: giữ phần làm chung và để class con điền phần còn thiếu.

```java
abstract class Greeting { // Không tạo new Greeting trực tiếp
    abstract String name(); // Class con phải cung cấp tên
    String text() { return "Hi " + name(); } // Phần dùng chung
}
class PersonalGreeting extends Greeting { // extends: kế thừa class cha
    String name() { return "Thinh"; }
}
System.out.println(new PersonalGreeting().text()); // Hi Thinh
```

**Khi nào dùng**

- Khi một nhóm class có luồng xử lý chung rõ ràng.
- Khi class con thực sự là một dạng của class cha.

**Lỗi hay gặp**

- Dùng extends chỉ để mượn vài dòng code.
- Gọi new trên abstract class.

**Quy tắc ngón tay cái:** Chung cách làm ổn định mới cân nhắc abstract class.

**Tự gõ, không dùng AI sinh code:** Tạo một class con khác trả tên bạn tự chọn. **Gợi ý:** Chỉ thay name; giữ phần ghép lời chào trong class cha.

## Composition Vs Inheritance

Composition là ghép object bằng cách cho một object dùng object khác; inheritance là kế thừa class cha qua extends.

```java
class Printer { void print(String text) { System.out.println(text); } }
class Report { // Report dùng Printer, không phải là Printer
    private final Printer printer;
    Report(Printer printer) { this.printer = printer; } // Nhận từ ngoài
    void show() { printer.print("Report ready"); }
}
new Report(new Printer()).show(); // Report ready
```

**Liên hệ FE:** Truyền công cụ qua constructor gần với truyền callback qua props: nơi dùng không phải tự chọn mọi hành vi.

**Khi nào dùng**

- Khi service (class xử lý nghiệp vụ) cần dùng một công cụ khác.
- Khi muốn chọn dependency (object được cần đến) từ ngoài.

**Lỗi hay gặp**

- Cho Report extends Printer chỉ để gọi print.
- Tưởng final printer làm Printer immutable.

**Quy tắc ngón tay cái:** “Dùng một” thường là composition; “là một dạng của” mới xét inheritance.

**Tự gõ, không dùng AI sinh code:** Đổi Printer thành interface rồi truyền một cách in khác vào Report. **Gợi ý:** Ghép lại cách viết ở phần Interface; constructor lưu object được truyền vào.

## `equals`, `hashCode` Và `toString`

### So sánh và hash

equals kiểm tra bằng nhau theo quy tắc class; hashCode là số giúp collection (bộ chứa nhiều phần tử) tìm kiếm nhanh.

```java
record ProductId(String value) {} // Record tự sinh equals/hashCode theo dữ liệu
ProductId first = new ProductId("p1");
ProductId second = new ProductId("p1");
System.out.println(first == second); // false: hai object
System.out.println(first.equals(second)); // true: cùng value
System.out.println(first.hashCode() == second.hashCode()); // true
```

**Khi nào dùng**

- Khi mã sản phẩm hoặc Money cần so theo giá trị.
- Khi dùng object làm khóa tìm kiếm.

**Lỗi hay gặp**

- Nghĩ class thường tự so các field như record.
- Tự viết equals nhưng quên hashCode.

**Quy tắc ngón tay cái:** Bằng nhau theo equals thì phải cùng hash; chiều ngược lại không bắt buộc.

**Tự gõ, không dùng AI sinh code:** Tạo hai ProductId khác value, ghi lại kết quả equals và hash. **Gợi ý:** Cùng hash không chứng minh bằng nhau; không dùng kết quả hash để thay equals.

### In object dễ đọc

toString trả văn bản mô tả object để bạn đọc kết quả hoặc tìm lỗi.

```java
class Label {
    @Override // Nhờ compiler kiểm tra việc ghi đè method có sẵn
    public String toString() { return "Label[Java]"; }
}
System.out.println(new Label()); // Label[Java]
```

**Khi nào dùng**

- Khi đọc log (thông tin chương trình ghi lại).
- Khi kiểm tra object trong lúc tìm lỗi.

**Lỗi hay gặp**

- So object bằng chuỗi toString.
- In mật khẩu hoặc token vào log.

**Quy tắc ngón tay cái:** toString để đọc, equals để so.

**Tự gõ, không dùng AI sinh code:** Tạo Label chứa tên riêng và in tên đó qua toString. **Gợi ý:** Thêm field và constructor; chưa cần equals ở bài nhỏ này.

## Immutability

Immutable là object không đổi dữ liệu sau khi tạo; muốn kết quả mới thì tạo object mới.

```java
record Counter(int value) { // int không chứa object có thể sửa từ ngoài
    Counter next() { return new Counter(value + 1); }
}
Counter oldCounter = new Counter(1);
Counter newCounter = oldCounter.next();
System.out.println(oldCounter.value()); // 1
System.out.println(newCounter.value()); // 2
```

**Liên hệ FE:** Gần với tạo state mới trong React thay vì sửa state đang dùng.

**Khi nào dùng**

- Khi dữ liệu biểu diễn một giá trị như Money.
- Khi muốn truyền object mà không lo nơi khác sửa nó.

**Lỗi hay gặp**

- Nghĩ final List chặn việc thêm phần tử.
- Trả trực tiếp list có thể sửa từ trong object.

**Quy tắc ngón tay cái:** Muốn giá trị mới thì trả object mới; kiểm tra cả dữ liệu lồng bên trong.

**Tự gõ, không dùng AI sinh code:** Viết Counter bằng class thường, giữ hành vi như ví dụ. **Gợi ý:** Dùng private final int, constructor và method trả object mới.

## Bài Tập Chính: ShoppingCart

ShoppingCart là bài tập ghép các object sau khi bạn hiểu từng phần riêng.

- Làm ProductId trước, rồi CartItem chứa mã và quantity (số lượng) dương.
- Cart chứa danh sách CartItem; thêm một item rồi tính tổng số lượng.
- Sau đó mới ghép Money để tính tiền.

**Gợi ý:** Chỉ làm một class mỗi buổi; đừng thêm quy tắc giảm giá trước khi tổng tiền đúng.

**Đạt khi:** Quantity bằng 0 bị chặn; dữ liệu cũ không bị sửa nếu bạn chọn cart immutable. Tự gõ phần triển khai, không dùng AI sinh lời giải.

## Hướng học tiếp theo

Làm [Money và Notification Sender](03-exercises.md) trước, ShoppingCart sau. Tiếp theo đọc [Generics cơ bản](../week-02/01-generics.md).

## Đọc thêm sau

- Tự viết equals(Object), hashCode: xem [Object API Java 21](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/lang/Object.html). equals cần đối xứng, bắc cầu, nhất quán và trả false khi so với null.
- Defensive copy là sao chép dữ liệu để bên ngoài không sửa nội bộ. List.copyOf chặn sửa cấu trúc list nhưng không sao chép sâu các phần tử.
- Abstract class có flow cố định, protected, default method và quy tắc equality khi kế thừa học sau khi làm được hai bài cơ bản.
