# Design Pattern Và Refactoring

**Cách chạy ví dụ:** dùng JDK 21 (bộ công cụ Java). Mở terminal, gõ `jshell`; gõ từng khối Java độc lập, dùng `/reset` trước khối tiếp theo và `/exit` để thoát. JShell chạy được khai báo class, method (hàm của Java) và lệnh trực tiếp, không cần khung main. Các comment `//` giải thích dòng code và kết quả cần quan sát.

## Builder

Builder là object gom các lựa chọn trước khi tạo object kết quả.

**Quy tắc ngón tay cái:** Nhiều tham số tùy chọn mới cân nhắc builder.

**Tự làm:** Liệt kê tham số bắt buộc và tùy chọn của bộ lọc task. **Gợi ý:** Với hai field đơn giản, constructor thường rõ hơn.

## Strategy

Strategy là một cách làm có thể thay thế qua cùng khả năng.

```java
java.util.function.UnaryOperator<String> trim = String::trim; // String → String
java.util.function.UnaryOperator<String> keep = text -> text;
System.out.println(trim.apply(" Java ")); // Java
System.out.println(keep.apply(" Java ").length()); // 6
```

**Khi nào dùng**

- Có nhiều cách chuẩn hóa.
- Caller cần chọn hành vi từ ngoài.

**Lỗi hay gặp**

- Tách strategy cho một phép tính không có biến thể.

**Quy tắc ngón tay cái:** Cùng việc, nhiều cách: cân nhắc strategy.

**Tự gõ, không dùng AI sinh code:** Thêm cách chuẩn hóa chữ thường và chọn dùng nó từ bên ngoài. **Gợi ý:** Không nhét if theo mọi biến thể vào nơi sử dụng.

## Factory

Factory là method hoặc object tập trung việc tạo object cho bên gọi.

```java
record Label(String value) {}
Label defaultLabel() { return new Label("TODO"); } // Tạo theo mặc định chung
System.out.println(defaultLabel().value()); // TODO
```

**Khi nào dùng**

- Tên method làm rõ ý định tạo object.
- Nhiều nơi cần cùng quy tắc khởi tạo.

**Lỗi hay gặp**

- Thêm factory chỉ để bọc new mà không giúp hiểu hơn.

**Tự gõ, không dùng AI sinh code:** Viết factory cho một nhãn khác cần kiểm tra dữ liệu. **Gợi ý:** Factory phải giúp tên gọi hoặc quy tắc tạo rõ hơn.

## Observer

Observer là bên đăng ký nhận thông báo khi có sự kiện xảy ra.

```java
java.util.function.Consumer<String> listener = text -> System.out.println("Event: " + text);
listener.accept("task-created"); // Consumer nhận dữ liệu, không trả kết quả
```

**Liên hệ FE:** Gần callback đăng ký vào event listener trong JS; ví dụ này gọi đồng bộ.

**Khi nào dùng**

- Một sự kiện cần báo nhiều bên.
- Bên nghe cần đăng ký hoặc gỡ đăng ký.

**Lỗi hay gặp**

- Nghĩ callback luôn chạy trên thread khác.
- Không quy định xử lý lỗi của listener.

**Tự gõ, không dùng AI sinh code:** Tạo hai listener và tự gọi cả hai khi task được tạo. **Gợi ý:** Chưa cần làm thư viện event tổng quát.

## Refactoring Workflow

Refactoring workflow là chuỗi bước đổi code có kiểm soát.

**Quy tắc ngón tay cái:** Một thay đổi nhỏ, một lần kiểm tra hành vi.

**Tự làm:** Đổi tên method rồi tách một helper trong hai bước riêng. **Gợi ý:** Nếu test hỏng, bạn biết thay đổi nào gây ra.

## Code Smell Hay Gặp

Code smell là dấu hiệu cần xem lại thiết kế, chưa phải bằng chứng code sai.

**Quy tắc ngón tay cái:** Mô tả khó khăn cụ thể trước khi chọn pattern.

**Tự làm:** Tìm đoạn xử lý title bị lặp ở hai nơi. **Gợi ý:** Kiểm xem hai đoạn có thực sự cùng quy tắc trước khi gộp.

## Bài Tập Nhanh

Sửa một điểm khó đọc trong TaskService.

- Viết kiểm tra trước.
- Đổi cấu trúc nhỏ.
- Ghi điều dễ hiểu hơn.

**Gợi ý:** Không thêm pattern chỉ để có tên pattern trong bài.

**Đạt khi:** Hành vi giữ nguyên, lý do sửa cụ thể. Tự gõ phần triển khai, không dùng AI sinh lời giải.

## Đọc thêm sau

Builder đầy đủ, observer có nhiều luồng và factory chọn theo cấu hình học sau. Design pattern là mẫu giải pháp quen thuộc, không phải mục tiêu cần phủ đủ trong project.
