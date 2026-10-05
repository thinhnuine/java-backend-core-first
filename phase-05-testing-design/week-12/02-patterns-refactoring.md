# Design Pattern Và Refactoring

## Builder

Hữu ích khi object có nhiều optional field.

```java
User user = User.builder()
    .id("u1")
    .email("a@example.com")
    .displayName("A")
    .build();
```

Không cần builder cho object chỉ có 2-3 field rõ ràng.

## Strategy

Hữu ích khi có nhiều thuật toán thay thế nhau.

Use case:

- discount policy.
- retry policy.
- pricing rule.
- sorting/ranking rule.

## Factory

Hữu ích khi logic tạo object phức tạp hoặc cần ẩn concrete type.

```java
public interface ParserFactory {
    Parser create(String contentType);
}
```

## Observer

Hữu ích cho event/listener. Bài Event Emitter là ví dụ tự nhiên.

## Refactoring Workflow

1. Viết hoặc bổ sung test bảo vệ behavior.
2. Chọn một mùi code cụ thể.
3. Refactor nhỏ.
4. Chạy test.
5. Ghi note tradeoff.

## Code Smell Hay Gặp

- Long method.
- Large class.
- Primitive obsession.
- Feature envy.
- Shotgun surgery.
- Duplicate logic.
- Boolean flag điều khiển quá nhiều branch.

## Bài Tập Nhanh

Chọn một class cũ, tìm 2 code smell và ghi:

- smell là gì.
- vì sao gây vấn đề.
- refactor nhỏ nhất là gì.
