# SOLID Thực Dụng

## Single Responsibility

Một class có một lý do chính để thay đổi.

Dấu hiệu vi phạm:

- class parse input, validate, persist, format output cùng lúc.
- test setup rất dài.
- sửa feature nhỏ phải đụng nhiều behavior không liên quan.

## Open/Closed

Thêm behavior mới ít sửa code cũ.

Ví dụ strategy:

```java
public interface DiscountPolicy {
    Money discountFor(Order order);
}
```

Thêm policy mới bằng class mới thay vì sửa `if/else` lớn.

## Liskov Substitution

Subtype thay base type mà không phá kỳ vọng.

Nếu subclass override method và throw exception bất ngờ hoặc đổi meaning, hierarchy có mùi.

## Interface Segregation

Interface nhỏ, đúng nhu cầu caller.

Tránh:

```java
interface UserService {
    void create();
    void delete();
    void exportCsv();
    void sendMarketingEmail();
}
```

Nếu caller chỉ cần read, tách read interface có thể rõ hơn.

## Dependency Inversion

High-level code phụ thuộc abstraction.

Ví dụ:

- `OrderService` phụ thuộc `PaymentGateway`.
- Implementation cụ thể là `StripePaymentGateway`.

## Khi Nào Đừng Refactor

Đừng refactor chỉ vì thấy pattern trong sách.

Refactor khi:

- test khó viết.
- duplication có ý nghĩa.
- class quá nhiều responsibility.
- thay đổi mới làm code cũ phình to.
- bug lặp lại vì design khó hiểu.
