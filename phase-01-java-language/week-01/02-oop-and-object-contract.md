# OOP Và Object Contract

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
