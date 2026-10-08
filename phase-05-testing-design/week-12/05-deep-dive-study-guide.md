# Mentor Guide Tuần 12: SOLID, Pattern Và Refactoring

## 1. Ý chính

Tuần này học thiết kế code dễ sửa. `SOLID` và design pattern không phải luật tôn giáo, mà là ngôn ngữ để nhận diện vấn đề và chọn cách refactor. Mục tiêu là dùng pattern khi nó giảm complexity thật, không phải để code trông "senior".

## 2. Giải thích code

Ví dụ Strategy:

```java
package dev.thinh.javacore;

public interface DiscountPolicy {
    Money discountFor(ShoppingCart cart);
}
```

Implementation:

```java
public final class NoDiscountPolicy implements DiscountPolicy {
    @Override
    public Money discountFor(ShoppingCart cart) {
        return new Money(0, "USD");
    }
}
```

Service dùng strategy:

```java
public final class CheckoutService {
    private final DiscountPolicy discountPolicy;

    public CheckoutService(DiscountPolicy discountPolicy) {
        this.discountPolicy = discountPolicy;
    }

    public Money discountFor(ShoppingCart cart) {
        return discountPolicy.discountFor(cart);
    }
}
```

Giải thích:

- `DiscountPolicy`: abstraction cho rule giảm giá.
- `NoDiscountPolicy`: một implementation.
- `CheckoutService` phụ thuộc interface, không phụ thuộc class cụ thể.
- Thay policy không cần sửa service.

## 3. Vì sao thiết kế như vậy

Nếu dùng `if/else` khắp nơi:

```java
if (customerType.equals("VIP")) {
    // Tính phần giảm giá VIP theo quy tắc làm tròn đã xác định.
} else if (customerType.equals("NEW")) {
    // Tính phần giảm giá NEW theo quy tắc làm tròn đã xác định.
}
```

Khi thêm rule mới, bạn phải sửa nhiều chỗ. Strategy gom từng rule thành class riêng và service chỉ gọi contract.

Nhưng nếu chỉ có một rule đơn giản và không có dấu hiệu thay đổi, tạo Strategy có thể là over-engineering.

## 4. Liên hệ với Frontend

Strategy giống việc truyền function/prop để thay behavior:

```tsx
<PriceCalculator discountStrategy={vipDiscount} />
```

Builder hơi giống tạo config object nhiều option:

```javascript
createUser({ email, displayName, timezone })
```

Nhưng Java dùng class/method rõ ràng hơn vì không có object literal linh hoạt như JS.

## 5. Khi nào dùng và không dùng

| Pattern/Nguyên tắc | Khi dùng | Khi tránh |
| --- | --- | --- |
| SRP | Class có nhiều lý do thay đổi | Tách quá nhỏ làm khó đọc |
| Strategy | Nhiều thuật toán thay thế | Chỉ có một rule đơn giản |
| Builder | Object nhiều optional field | Object 2-3 field |
| Factory | Logic tạo object phức tạp | `new` rõ ràng là đủ |
| Observer | Event/listener | Flow sync đơn giản |

## 6. Bẫy hay gặp

- Tạo interface cho mọi class.
- Dùng pattern trước khi có vấn đề.
- Refactor không có test.
- Tách class quá nhỏ, flow bị vỡ vụn.
- Đổi public API mà không ghi lý do.

## 7. Thuật ngữ mới

- `SOLID`: nhóm nguyên tắc thiết kế OOP.
- `Strategy`: pattern tách thuật toán thành object thay thế được.
- `Builder`: pattern tạo object nhiều option.
- `Factory`: pattern gom logic tạo object.
- `Observer`: pattern event/listener.
- `refactoring`: đổi cấu trúc code mà giữ behavior.

## 8. Bài tập nhỏ để tự gõ lại

1. Chọn `LogProcessor`, `LruCache` hoặc `EventEmitter`.
2. Viết test trước.
3. Ghi 2 code smell.
4. Refactor một phần nhỏ.
5. Chạy test.
6. Viết `refactoring-notes.md`: vấn đề, thay đổi, tradeoff.

Học tiếp theo: mỗi lần muốn dùng pattern, viết một câu "pattern này giảm đau ở đâu?".

NoDiscountPolicy trong snippet dùng USD để minh họa và chỉ áp dụng cart USD. Với cart nhiều currency, lấy currency từ total của cart; không trả Money(0, "USD") cố định cho mọi cart. Money ở lab chưa có divide; tính phần trăm là bài mở rộng phải xác định rounding và overflow, không gọi method chưa được định nghĩa.
