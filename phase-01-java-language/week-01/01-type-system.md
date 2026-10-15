# Type System Trong Java

> **Làm rõ trước khi học:** final chỉ chặn gán lại biến, không tự làm object immutable. Không suy ra primitive luôn nằm trên stack hay object luôn được cấp phát riêng từ ví dụ này.

Mỗi phần là một lượt học 10–15 phút. Dừng sau khi tự giải thích được ví dụ.

**Cách chạy ví dụ:** dùng JDK 21 (bộ công cụ Java). Mở terminal, gõ `jshell`; gõ từng khối Java độc lập, dùng `/reset` trước khối tiếp theo và `/exit` để thoát. JShell chạy được khai báo class, method (hàm của Java) và lệnh trực tiếp, không cần khung main. Các comment `//` giải thích dòng code và kết quả cần quan sát.

## Primitive Vs Reference Type

Primitive là kiểu giữ giá trị đơn giản; reference là giá trị tham chiếu tới object (đối tượng dữ liệu), giúp bạn phân biệt sao chép số với dùng chung object.

```java
int first = 10; // Primitive: số nguyên
int second = first; // Sao chép giá trị 10
second = 20;
int[] scores = {10}; // Array: object chứa các số
int[] sameScores = scores; // Cùng tham chiếu một array
sameScores[0] = 20; // Sửa object dùng chung
System.out.println(first); // 10
System.out.println(scores[0]); // 20
```

**Liên hệ FE:** Array trong JS cũng được dùng chung sau phép gán, trừ khi bạn chủ động sao chép.

**Khi nào dùng**

- Dùng int/long cho số nguyên bắt buộc có giá trị.
- Dùng String, array hoặc class khi cần object.

**Lỗi hay gặp**

- Nghĩ gán reference sẽ sao chép object.
- Gán null (không trỏ tới object nào) cho int.

**Quy tắc ngón tay cái:** Phép gán sao chép giá trị của biến; với reference, giá trị được sao chép là tham chiếu.

**Tự gõ, không dùng AI sinh code:** Tạo hai biến số rồi hai biến cùng giữ một array; đổi giá trị và đoán kết quả. **Gợi ý:** Thay tên và số trong ví dụ; so sánh gán biến với sửa phần tử.

## Boxing Và Unboxing

Boxing là chuyển primitive thành wrapper (object bọc giá trị); unboxing là lấy giá trị primitive ra để tính toán.

```java
Integer wrapped = 7; // Boxing: int thành Integer
int count = wrapped; // Unboxing: Integer thành int
System.out.println(count + 1); // 8
```

**Khi nào dùng**

- Dùng Integer khi API cần object.
- Dùng int nếu số luôn có mặt và không cần object.

**Lỗi hay gặp**

- Unboxing một wrapper null gây NullPointerException (gọi dữ liệu không tồn tại).
- Dùng == để so giá trị hai Integer.

**Quy tắc ngón tay cái:** Ưu tiên primitive cho số bắt buộc; chỉ dùng wrapper khi có lý do.

**Tự gõ, không dùng AI sinh code:** Thử một Integer hợp lệ rồi một Integer null; đọc lỗi khi gán sang int. **Gợi ý:** Chạy hai trường hợp riêng để nhìn đúng dòng gây lỗi.

## `String` Và Immutability

String là kiểu văn bản immutable (nội dung object không đổi sau khi tạo), giúp việc truyền chuỗi giữa các nơi dễ dự đoán.

```java
String title = " Java ";
String cleaned = title.trim(); // Trả chuỗi đã bỏ khoảng trắng hai đầu
System.out.println(title.length()); // 6: chuỗi cũ giữ nguyên
System.out.println(cleaned); // Java
System.out.println("Java".equals(cleaned)); // true: so nội dung
```

**Liên hệ FE:** String primitive trong JS so bằng === theo nội dung; Java String là object nên đừng chuyển nguyên thói quen đó sang ==.

**Khi nào dùng**

- Dùng String cho tên, mã và thông báo.
- Dùng equals khi so nội dung chuỗi.

**Lỗi hay gặp**

- Gọi trim nhưng không giữ kết quả.
- Dùng == (so cùng object) thay cho equals.

**Quy tắc ngón tay cái:** Chuỗi cần nội dung bằng nhau: dùng equals.

**Tự gõ, không dùng AI sinh code:** Tạo hai chuỗi cùng nội dung bằng new String rồi so == và equals. **Gợi ý:** In riêng hai kết quả và giải thích mỗi câu hỏi đang được hỏi.

## Package, Class Và Access Modifier

Class gom dữ liệu và method; package đặt tên nhóm class; access modifier là từ khóa giới hạn nơi được truy cập.

```java
class Counter { // Class: bản mô tả object
    private int value = 0; // Chỉ code trong phạm vi class được sửa trực tiếp
    public int value() { return value; } // Method public cho bên ngoài đọc
}
Counter counter = new Counter(); // Tạo object
System.out.println(counter.value()); // 0
```

**Khi nào dùng**

- Dùng private cho dữ liệu nội bộ.
- Dùng public cho hành vi bên ngoài cần gọi.

**Lỗi hay gặp**

- Cho mọi field public.
- Trong project, đặt package không khớp tổ chức thư mục.

**Quy tắc ngón tay cái:** Field private, method public khi có người bên ngoài cần dùng.

**Tự gõ, không dùng AI sinh code:** Tạo Counter.java trong package dev.thinh.javacore bằng IDE rồi thêm method tăng giá trị. **Gợi ý:** Dùng New → Package, rồi New → Java Class; dòng package ở đầu file, không gõ vào JShell.

## Bài Tập Chính: `Money`

Money là bài gom số tiền và đơn vị tiền vào một object có quy tắc rõ.

- Tạo amount kiểu long, currency kiểu String; constructor (hàm khởi tạo object) nhận hai giá trị.
- Từ chối amount âm, currency null hoặc toàn khoảng trắng.
- Viết add nhận Money cùng currency và trả Money mới.

**Gợi ý:** Dùng private final; final ngăn gán lại field sau khởi tạo. Kiểm tra đầu vào trước khi gán this.amount.

**Đạt khi:** Cộng 100 và 20 cùng USD được 120; object ban đầu vẫn là 100. Khác currency bị từ chối. Tự gõ phần triển khai, không dùng AI sinh lời giải.

## Hướng học tiếp theo

Đọc [OOP và object contract](02-oop-and-object-contract.md) sau khi tạo và gọi được Money. Chưa cần làm ShoppingCart ngay.

## Đọc thêm sau

- Quy ước bài Money: amount tính theo đơn vị nhỏ nhất đã chọn; không dùng double cho phép tính cần chính xác theo đơn vị đó. Math.addExact phát hiện overflow (kết quả vượt giới hạn long).
- String pooling (tái dùng object chuỗi), bộ nhớ và tối ưu của JVM học sau.
- Xem [bài tập tuần 1](03-exercises.md) để hoàn thiện quy tắc tiền và lỗi.
