# Mentor Guide Tuần 11: Testing Với JUnit, AssertJ Và Mockito

## 1. Ý chính

Testing trong Java không chỉ để đạt coverage. Test giúp bạn khóa behavior để refactor tự tin. Tuần này bạn học `JUnit 5`, `AssertJ`, `Mockito`, parameterized test và cách viết test đọc như tài liệu behavior.

## 2. Giải thích code

Ví dụ test `Money`:

```java
package dev.thinh.javacore;

import org.junit.jupiter.api.Test;

import static org.assertj.core.api.Assertions.assertThat;
import static org.assertj.core.api.Assertions.assertThatThrownBy;

class MoneyTest {
    @Test
    void addsMoneyWithSameCurrency() {
        Money a = new Money(100, "USD");
        Money b = new Money(50, "USD");

        Money result = a.add(b);

        assertThat(result.amount()).isEqualTo(150);
        assertThat(result.currency()).isEqualTo("USD");
    }

    @Test
    void rejectsDifferentCurrency() {
        Money usd = new Money(100, "USD");
        Money vnd = new Money(100, "VND");

        assertThatThrownBy(() -> usd.add(vnd))
            .isInstanceOf(IllegalArgumentException.class)
            .hasMessageContaining("currency");
    }
}
```

Giải thích:

- `@Test`: đánh dấu method là test.
- `assertThat`: AssertJ assertion đọc tự nhiên.
- `assertThatThrownBy`: kiểm tra exception.
- Test name `addsMoneyWithSameCurrency`: mô tả behavior.
- Arrange: tạo `a`, `b`.
- Act: gọi `a.add(b)`.
- Assert: kiểm result.

## 3. Vì sao thiết kế như vậy

Test nên kiểm behavior, không kiểm implementation detail.

Test xấu:

```java
assertThat(cache.internalMap().size()).isEqualTo(2);
```

Nếu sau này bạn đổi implementation không dùng map nữa, test fail dù behavior đúng.

Test tốt hơn:

```java
cache.put("a", 1);
cache.put("b", 2);
cache.put("c", 3);

assertThat(cache.get("a")).isEmpty();
```

Nó kiểm behavior eviction, không khóa internal structure.

## 4. Liên hệ với Frontend

Nếu bạn từng dùng Jest/React Testing Library, tư duy giống nhau: test behavior người dùng/consumer thấy, không test internal state quá sâu.

React Testing Library khuyên test như user dùng app. Java unit test cũng nên test public API của class.

## 5. Khi nào dùng và không dùng

| Công cụ | Khi dùng | Khi tránh |
| --- | --- | --- |
| JUnit `@Test` | Mọi unit test cơ bản | Không có |
| AssertJ | Assertion collection/exception/object | Team không dùng dependency này |
| Parameterized test | Cùng behavior nhiều input | Test quá khác nhau |
| Mockito | Dependency khó dựng/gọi external | Value object/class đơn giản |
| Fake | Dependency đơn giản tự viết được | Fake phức tạp hơn mock |

## 6. Bẫy hay gặp

- Test name mơ hồ như `testAdd`.
- Mock quá nhiều.
- Test private method.
- Assertion quá chung chung.
- Không test edge case.
- Test phụ thuộc thứ tự ngẫu nhiên.

## 7. Thuật ngữ mới

- `unit test`: test một unit nhỏ như class/method.
- `assertion`: điều kiện test kỳ vọng đúng.
- `mock`: object giả do framework tạo.
- `fake`: implementation giả tự viết.
- `behavior`: hành vi quan sát từ public API.
- `parameterized test`: test chạy nhiều input.

## 8. Bài tập nhỏ để tự gõ lại

1. Viết `MoneyTest`.
2. Test constructor reject amount âm.
3. Test currency blank/null.
4. Test add cùng currency.
5. Test add khác currency.
6. Viết parameterized test cho blank currency.

Học tiếp theo: nếu test khó viết, đừng chỉ trách test; có thể design class đang khó dùng.
