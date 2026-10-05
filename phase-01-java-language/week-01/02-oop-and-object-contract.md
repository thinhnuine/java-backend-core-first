# OOP Và Object Contract

## Cách Học File Này

File này không yêu cầu bạn thuộc định nghĩa OOP. Mục tiêu là biết chọn công cụ đúng:

- Khi nào tạo interface?
- Khi nào abstract class đáng dùng?
- Khi nào inheritance làm code rối?
- Vì sao `equals/hashCode` là contract, không phải boilerplate?
- Immutable object giúp giảm bug thế nào?

Đọc xong mỗi section, hãy tự hỏi: "Nếu codebase lớn lên gấp 10 lần, lựa chọn này còn dễ sửa không?"

## 1. Interface

Interface mô tả capability hoặc contract.

```java
public interface NotificationSender {
    void send(String recipient, String message);
}
```

Class implement interface:

```java
public class EmailNotificationSender implements NotificationSender {
    @Override
    public void send(String recipient, String message) {
        System.out.println("Send email to " + recipient + ": " + message);
    }
}
```

Dùng interface khi:

- Có nhiều implementation.
- Muốn tách code gọi khỏi chi tiết implementation.
- Muốn test dễ bằng fake/mock.

### Interface Không Phải Chỉ Để Cho Đẹp

Nếu chỉ có đúng một class và chưa có nhu cầu thay implementation, interface có thể chưa cần.

Ví dụ chưa cần interface:

```java
public class SlugGenerator {
    public String generate(String title) {
        return title.toLowerCase().replace(" ", "-");
    }
}
```

Ví dụ nên có interface:

```java
public interface PaymentGateway {
    void charge(Money amount);
}
```

Vì sau này có thể có:

- `StripePaymentGateway`
- `PaypalPaymentGateway`
- `FakePaymentGateway` trong test

### So Với TypeScript Interface

TypeScript interface chủ yếu là compile-time structural typing. Java interface là nominal typing: class phải khai báo `implements InterfaceName`.

TypeScript:

```ts
type Sender = {
  send(message: string): void
}
```

Java:

```java
public interface Sender {
    void send(String message);
}

public class EmailSender implements Sender {
    @Override
    public void send(String message) {
        System.out.println(message);
    }
}
```

Java rõ ràng hơn, nhưng verbose hơn.

## 2. Abstract Class

Abstract class dùng khi các subclass có phần state hoặc behavior chung.

```java
public abstract class FileImporter {
    public final void importFile(String path) {
        validate(path);
        parse(path);
    }

    private void validate(String path) {
        if (path == null || path.isBlank()) {
            throw new IllegalArgumentException("path is required");
        }
    }

    protected abstract void parse(String path);
}
```

Dùng abstract class khi:

- Có template flow chung.
- Có logic chung thật sự cần chia sẻ.
- Subclass chỉ thay đổi một vài bước.

Nếu chỉ cần contract, ưu tiên interface.

### Vì Sao `importFile` Là `final`

Trong ví dụ:

```java
public final void importFile(String path) {
    validate(path);
    parse(path);
}
```

`final` nghĩa là subclass không được override flow tổng. Subclass chỉ được thay bước `parse`. Đây là template method pattern ở mức nhỏ.

Nếu không cẩn thận, abstract class dễ làm hierarchy cứng. Bạn chỉ nên dùng khi có flow/state chung thật sự rõ.

### Câu Hỏi Trước Khi Dùng Abstract Class

- Các subclass có thật sự chia sẻ state/flow không?
- Hay mình chỉ muốn ép method contract? Nếu vậy interface đủ.
- Subclass có thể phá invariant của parent không?
- Composition có đơn giản hơn không?

## 3. Composition Vs Inheritance

Inheritance hỏi: "A có phải là B không?"

Composition hỏi: "A có B để làm việc không?"

Ví dụ nên dùng composition:

```java
public class OrderService {
    private final PaymentGateway paymentGateway;

    public OrderService(PaymentGateway paymentGateway) {
        this.paymentGateway = paymentGateway;
    }

    public void checkout(Order order) {
        paymentGateway.charge(order.totalAmount());
    }
}
```

Quy tắc thực dụng:

- Ưu tiên composition.
- Dùng inheritance khi quan hệ "is-a" rõ và ổn định.
- Tránh kế thừa chỉ để reuse vài dòng code.

### Ví Dụ Inheritance Dễ Sai

Giả sử bạn có:

```java
public class User {
    private final String email;
}

public class AdminUser extends User {
}
```

Ban đầu nghe hợp lý: admin là user. Nhưng sau đó business đổi:

- user có thể có nhiều role
- admin cũng có permission theo organization
- một user có thể vừa admin vừa billing manager

Inheritance bắt đầu cứng. Composition thường tốt hơn:

```java
public final class User {
    private final String email;
    private final Set<Role> roles;
}
```

### Rule Thực Dụng

Nếu bạn chỉ muốn "dùng lại code", đừng vội inheritance.

Hỏi:

```text
Object A có thật sự là một loại của B không?
Hay A chỉ đang dùng B để làm việc?
```

`OrderService` không phải là `PaymentGateway`. Nó có `PaymentGateway`. Đó là composition.

## 4. `equals` Và `hashCode`

Nếu object dùng trong `HashMap`, `HashSet`, hoặc cần so sánh theo giá trị, bạn phải hiểu `equals/hashCode`.

Contract:

- Nếu `a.equals(b)` là `true`, thì `a.hashCode() == b.hashCode()` phải đúng.
- Nếu `hashCode` giống nhau, `equals` chưa chắc true.
- `equals` nên reflexive, symmetric, transitive, consistent.

Ví dụ:

```java
import java.util.Objects;

public final class UserId {
    private final String value;

    public UserId(String value) {
        if (value == null || value.isBlank()) {
            throw new IllegalArgumentException("value is required");
        }
        this.value = value;
    }

    public String value() {
        return value;
    }

    @Override
    public boolean equals(Object other) {
        if (this == other) {
            return true;
        }
        if (!(other instanceof UserId userId)) {
            return false;
        }
        return Objects.equals(value, userId.value);
    }

    @Override
    public int hashCode() {
        return Objects.hash(value);
    }

    @Override
    public String toString() {
        return "UserId[value=" + value + "]";
    }
}
```

### Vì Sao Contract Này Quan Trọng

`HashMap` và `HashSet` dùng `hashCode` để tìm bucket, rồi dùng `equals` để xác định object có bằng nhau không.

Nếu bạn override `equals` mà quên `hashCode`, object có thể hành xử rất kỳ lạ trong `HashSet`.

Ví dụ:

```java
Set<UserId> ids = new HashSet<>();
ids.add(new UserId("u1"));

System.out.println(ids.contains(new UserId("u1")));
```

Bạn kỳ vọng `true`. Nếu `equals/hashCode` sai, có thể ra `false`.

### Khi Nào Cần Override

Cần override khi object là value object:

- `UserId`
- `ProductId`
- `Money`
- `EmailAddress`

Không phải lúc nào entity cũng cần override theo mọi field. Với JPA entity sau này, câu chuyện phức tạp hơn, nên hiện tại chỉ tập trung value object.

### `toString` Để Debug

`toString` nên chứa thông tin hữu ích nhưng không lộ secret.

Tốt:

```java
Money[amount=100,currency=USD]
```

Không tốt:

```java
dev.thinh.Money@1a2b3c
```

Không nên in password/token.

## 5. Immutability

Immutable object không đổi state sau khi tạo.

Checklist:

- Class `final` nếu không muốn subclass phá invariant.
- Field `private final`.
- Không expose mutable object trực tiếp.
- Validate ở constructor.
- Method thay đổi state trả object mới.

Ví dụ:

```java
public final class Counter {
    private final int value;

    public Counter(int value) {
        this.value = value;
    }

    public Counter increment() {
        return new Counter(value + 1);
    }

    public int value() {
        return value;
    }
}
```

### Mutable Vs Immutable

Mutable:

```java
public class MutableCounter {
    private int value;

    public void increment() {
        value++;
    }
}
```

Immutable:

```java
public final class Counter {
    private final int value;

    public Counter increment() {
        return new Counter(value + 1);
    }
}
```

Mutable object không xấu. Nhưng mutable state làm code khó đoán hơn, nhất là khi object được truyền qua nhiều layer hoặc nhiều thread cùng dùng.

### Defensive Copy

Nếu field là collection, `final` chưa đủ.

```java
public final class Tags {
    private final List<String> values;

    public Tags(List<String> values) {
        this.values = List.copyOf(values);
    }

    public List<String> values() {
        return values;
    }
}
```

`List.copyOf` tạo list không sửa được và tránh caller giữ reference mutable để đổi state bên trong.

### Khi Nào Ưu Tiên Immutable

- Value object.
- DTO/read model.
- Config.
- Object truyền qua thread.
- Object dùng làm key trong map/set.

Entity có lifecycle dài, ví dụ `Order`, đôi khi mutable hợp lý hơn. Nhưng với core Java giai đoạn đầu, tập viết immutable value object là bài tập rất tốt.

## 6. Bài Tập Nhanh

Thiết kế domain nhỏ cho `ShoppingCart`:

- `ProductId`
- `CartItem`
- `ShoppingCart`
- `DiscountPolicy` interface

Yêu cầu:

- Dùng composition.
- Ít nhất 2 class immutable.
- Override `equals/hashCode` cho value object.
- Viết `toString` hữu ích để debug.

## 7. Hướng Dẫn Làm Bài ShoppingCart Theo Bước

### Bước 1: Tạo `ProductId`

Đây là value object:

```java
public final class ProductId {
    private final String value;
}
```

Yêu cầu:

- validate `value` không null/blank
- accessor `value()`
- `equals/hashCode`
- `toString`

### Bước 2: Tạo `CartItem`

`CartItem` nên chứa:

- `ProductId productId`
- `String name`
- `int quantity`
- `Money unitPrice`

Rule:

- quantity > 0
- name không blank
- unitPrice không null

### Bước 3: Tạo `ShoppingCart`

Ban đầu bạn có thể chọn mutable hoặc immutable. Để học immutability, thử immutable:

```java
public final class ShoppingCart {
    private final List<CartItem> items;

    public ShoppingCart(List<CartItem> items) {
        this.items = List.copyOf(items);
    }

    public ShoppingCart addItem(CartItem item) {
        List<CartItem> newItems = new ArrayList<>(items);
        newItems.add(item);
        return new ShoppingCart(newItems);
    }
}
```

Bạn sẽ cần import:

```java
import java.util.ArrayList;
import java.util.List;
```

### Bước 4: Tạo `DiscountPolicy`

Interface:

```java
public interface DiscountPolicy {
    Money discountFor(ShoppingCart cart);
}
```

Sau đó có thể tạo:

```java
public final class NoDiscountPolicy implements DiscountPolicy {
    @Override
    public Money discountFor(ShoppingCart cart) {
        return new Money(0, "USD");
    }
}
```

Sau này bạn sẽ thấy đây là Strategy pattern.

## 8. Lỗi Thường Gặp

### Dùng Inheritance Chỉ Để Reuse Code

Nếu lý do duy nhất là "muốn dùng lại vài method", hãy cân nhắc composition hoặc helper class.

### Interface Quá To

Sai:

```java
public interface UserManager {
    void create();
    void delete();
    void exportCsv();
    void sendEmail();
}
```

Interface càng to càng khó implement và test.

### Override `equals` Nhưng Quên `hashCode`

Nếu override một cái, gần như luôn phải override cả hai.

### Immutable Nhưng Expose Mutable List

Sai:

```java
public List<CartItem> items() {
    return items;
}
```

nếu `items` là mutable list nội bộ. Dùng `List.copyOf` hoặc trả unmodifiable view.

## 9. Câu Hỏi Tự Kiểm Tra

- Interface khác abstract class ở đâu?
- Khi nào abstract class đáng dùng?
- Vì sao composition thường linh hoạt hơn inheritance?
- `equals/hashCode` ảnh hưởng gì tới `HashSet`?
- `toString` tốt giúp debug thế nào?
- Class immutable cần những điều kiện gì?
- Vì sao `final List<T>` chưa chắc immutable?
