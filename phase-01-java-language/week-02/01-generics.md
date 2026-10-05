# Generics Trong Java

## 1. Vì Sao Cần Generics

Generics giúp type-safe khi class hoặc method làm việc với nhiều type.

Không generic:

```java
public class Box {
    private Object value;

    public Object get() {
        return value;
    }

    public void set(Object value) {
        this.value = value;
    }
}
```

Vấn đề: caller phải cast, lỗi có thể nổ ở runtime.

Generic:

```java
public class Box<T> {
    private T value;

    public T get() {
        return value;
    }

    public void set(T value) {
        this.value = value;
    }
}
```

## 2. Generic Method

```java
public final class Lists {
    private Lists() {
    }

    public static <T> T first(List<T> values) {
        if (values == null || values.isEmpty()) {
            throw new IllegalArgumentException("values must not be empty");
        }
        return values.get(0);
    }
}
```

## 3. Bounded Type

```java
public static <T extends Comparable<T>> T max(List<T> values) {
    if (values == null || values.isEmpty()) {
        throw new IllegalArgumentException("values must not be empty");
    }

    T result = values.get(0);
    for (T value : values) {
        if (value.compareTo(result) > 0) {
            result = value;
        }
    }
    return result;
}
```

`T extends Comparable<T>` nghĩa là T phải có khả năng compare với chính nó.

## 4. Wildcard

Wildcard `?` dùng khi bạn không cần hoặc không thể biết type cụ thể.

```java
public static void printAll(List<?> values) {
    for (Object value : values) {
        System.out.println(value);
    }
}
```

`List<?>` đọc được item như `Object`, nhưng không add item cụ thể được, trừ `null`.

## 5. PECS

PECS: Producer Extends, Consumer Super.

Nếu collection là nguồn đọc ra T, dùng `? extends T`.

```java
public static double total(List<? extends Number> numbers) {
    double sum = 0;
    for (Number number : numbers) {
        sum += number.doubleValue();
    }
    return sum;
}
```

Nếu collection là nơi ghi T vào, dùng `? super T`.

```java
public static void addDefaults(List<? super Integer> values) {
    values.add(1);
    values.add(2);
    values.add(3);
}
```

## 6. Type Erasure

Java generics chủ yếu tồn tại ở compile time. Khi runtime, nhiều thông tin generic bị xóa.

Không thể làm:

```java
// if (value instanceof List<String>) { } // không hợp lệ
// T item = new T();                      // không hợp lệ
```

Hệ quả:

- Generic giúp compiler bắt lỗi sớm.
- Runtime không luôn biết `List<String>` hay `List<Integer>`.
- Khi cần type runtime, thường truyền thêm `Class<T>` hoặc type token.

## 7. Bài Tập Nhanh

Viết `Repository<ID, T>`:

- `void save(ID id, T value)`.
- `Optional<T> findById(ID id)`.
- `List<T> findAll()`.
- `boolean existsById(ID id)`.
- `void deleteById(ID id)`.

Sau đó implement `InMemoryRepository<ID, T>` bằng `HashMap`.
