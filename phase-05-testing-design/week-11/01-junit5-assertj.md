# JUnit 5 Và AssertJ

## JUnit 5 Basics

```java
class MoneyTest {
    @Test
    void addsMoneyWithSameCurrency() {
        Money result = new Money(10, "USD").add(new Money(5, "USD"));

        assertEquals(new Money(15, "USD"), result);
    }
}
```

Tên test nên mô tả behavior.

## Arrange, Act, Assert

Một test dễ đọc thường có 3 phần:

- Arrange: chuẩn bị input/dependency.
- Act: gọi behavior đang test.
- Assert: kiểm kết quả.

```java
@Test
void rejectsNegativeAmount() {
    assertThatThrownBy(() -> new Money(-1, "USD"))
        .isInstanceOf(IllegalArgumentException.class)
        .hasMessageContaining("amount");
}
```

## AssertJ

AssertJ giúp assertion đọc tự nhiên hơn:

```java
assertThat(users)
    .extracting(User::email)
    .containsExactly("a@example.com", "b@example.com");
```

Hay dùng:

- `isEqualTo`.
- `containsExactly`.
- `containsExactlyInAnyOrder`.
- `extracting`.
- `isEmpty`.
- `hasSize`.
- `assertThatThrownBy`.

## Test Behavior

Test nên kiểm behavior public, không khóa implementation quá chặt.

Ví dụ tốt:

- LRU cache evict đúng key.
- Parser trả AST đúng.
- Service gọi dependency khi cần.

Ví dụ dễ brittle:

- Kiểm private method.
- Kiểm internal list class cụ thể.
- Verify interaction không quan trọng.

## Bài Tập Nhanh

Thêm test cho `Money`, `LruCache` hoặc `LogLineParser`:

- happy path.
- invalid input.
- edge case.
- exception message đủ rõ.
