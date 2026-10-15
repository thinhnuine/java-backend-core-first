# SOLID Thực Dụng

**Cách chạy ví dụ:** dùng JDK 21 (bộ công cụ Java). Mở terminal, gõ `jshell`; gõ từng khối Java độc lập, dùng `/reset` trước khối tiếp theo và `/exit` để thoát. JShell chạy được khai báo class, method (hàm của Java) và lệnh trực tiếp, không cần khung main. Các comment `//` giải thích dòng code và kết quả cần quan sát.

## Single Responsibility

Single Responsibility là giữ một nhóm trách nhiệm rõ trong mỗi phần code để việc sửa có phạm vi dễ hiểu.

**Quy tắc ngón tay cái:** Một lý do thay đổi rõ hơn một class làm mọi thứ.

**Tự làm:** Đánh dấu code kiểm title và code gửi HTTP response trong TaskService. **Gợi ý:** Chuyển phần HTTP về nơi nhận request; không chia mỗi dòng thành một class.

## Open/Closed

Open/Closed là thiết kế để thêm một cách làm mới mà ít sửa phần đang dùng các cách làm đó.

```java
interface PriceRule { int apply(int amount); }
PriceRule fullPrice = amount -> amount; // Một cách tính
PriceRule discount = amount -> amount - 10; // Cách tính khác, số mẫu nhỏ
System.out.println(discount.apply(100)); // 90
```

**Khi nào dùng**

- Khi cần thao tác này trong bài thực hành.
- Khi đọc code có cùng cách viết.

**Lỗi hay gặp**

- Dùng trước khi hiểu kiểu đầu vào và kết quả.

**Quy tắc ngón tay cái:** Có biến thể thật rồi mới tách điểm thay thế.

**Tự gõ, không dùng AI sinh code:** Thêm quy tắc giảm theo phần trăm với số nguyên chia hết. **Gợi ý:** Nêu riêng cách xử lý làm tròn nếu kết quả không nguyên.

## Liskov Substitution

Liskov Substitution yêu cầu class con dùng thay class cha mà không phá lời hứa hành vi của class cha.

**Quy tắc ngón tay cái:** Class con phải giữ điều bên gọi đang tin cậy.

**Tự làm:** Một kho dữ liệu cam kết save nhưng class con luôn ném lỗi có thay thế được không? **Gợi ý:** Xét hợp đồng thực tế, không chỉ xem code có compile.

## Interface Segregation

Interface Segregation là tách interface theo khả năng bên dùng cần, tránh buộc implementation cung cấp việc không liên quan.

**Quy tắc ngón tay cái:** Bên chỉ đọc không cần biết mọi thao tác ghi.

**Tự làm:** Tách nhu cầu đọc task và gửi thông báo. **Gợi ý:** Đừng tạo nhiều interface nếu chưa có bên dùng khác nhau.

## Dependency Inversion

Dependency Inversion cho code nghiệp vụ phụ thuộc khả năng cần dùng, thay vì gắn với một công cụ cụ thể.

```java
interface TitleStore { String find(); }
class TaskService {
    private final TitleStore store;
    TaskService(TitleStore store) { this.store = store; }
    String title() { return store.find(); }
}
System.out.println(new TaskService(() -> "Java").title()); // Java
```

**Khi nào dùng**

- Khi cần thao tác này trong bài thực hành.
- Khi đọc code có cùng cách viết.

**Lỗi hay gặp**

- Dùng trước khi hiểu kiểu đầu vào và kết quả.

**Quy tắc ngón tay cái:** Service nói cần gì; nơi khởi tạo chọn cách cung cấp.

**Tự gõ, không dùng AI sinh code:** Truyền một store trả title khác mà không sửa TaskService. **Gợi ý:** Lambda là cách ngắn cung cấp method duy nhất của interface.

## Khi Nào Đừng Refactor

Refactor là đổi cấu trúc code mà giữ hành vi hiện có.

**Quy tắc ngón tay cái:** Chưa có vấn đề cụ thể thì chưa cần thêm tầng thiết kế.

**Tự làm:** Chọn một method khó đọc và nêu điều muốn cải thiện. **Gợi ý:** Viết kiểm tra hành vi trước; thay đổi nhỏ rồi chạy lại.

## Đọc thêm sau

SOLID là tên nhóm năm nguyên tắc trên, không phải danh sách buộc mọi class phải tách thành interface. Học quan hệ thay thế phức tạp và kiến trúc nhiều tầng khi project có nhu cầu.
