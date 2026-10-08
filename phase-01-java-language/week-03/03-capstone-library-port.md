# Capstone: Port Một Thư Viện Nhỏ Sang Java

> **Tự chọn sau nền collections.** Project bắt buộc theo lịch mới là [Task Manager](../../learning-path/03-task-manager-project.md). Không cần tự viết parser/LRU trước khi bắt đầu HTTP và Spring.

## Mục Tiêu

Bạn chọn một thư viện nhỏ từng quen ở JS/Python, rồi viết lại bằng Java. Mục tiêu không phải làm nhiều feature, mà là luyện API design, type system, OOP, generics, exception và test.

## Chọn 1 Trong 3 Đề

### Option A: JSON Parser Đơn Giản

Scope:

- Parse object: `{ "name": "Java" }`.
- Parse array: `[1, 2, 3]`.
- Parse string, number, boolean, null.
- Không cần full JSON spec ngay từ đầu.

Java concepts nên dùng:

- Sealed interface cho `JsonValue`.
- Record cho `JsonString`, `JsonNumber`, `JsonBoolean`, `JsonNull`, `JsonArray`, `JsonObject`.
- Custom exception `JsonParseException`.
- Text block cho test sample.

Public API gợi ý:

```java
JsonValue value = JsonParser.parse("""
    { "name": "Java", "version": 21 }
    """);
```

### Option B: LRU Cache

Scope:

- Generic `LruCache<K, V>`.
- `put`, `get`, `containsKey`, `size`, `clear`.
- Evict least recently used item khi quá capacity.

Java concepts nên dùng:

- Generics.
- `Optional<V>`.
- `HashMap`.
- Có thể tự viết doubly linked list để học sâu.
- Override `toString` để debug state.

Public API gợi ý:

```java
LruCache<String, Integer> cache = new LruCache<>(2);
cache.put("a", 1);
cache.put("b", 2);
cache.get("a");
cache.put("c", 3); // evict b
```

### Option C: Event Emitter

Scope:

- Subscribe listener theo event name.
- Emit event với payload.
- Unsubscribe.
- Listener chạy theo thứ tự đăng ký.

Java concepts nên dùng:

- Generic payload type nếu muốn.
- Functional interface.
- Composition.
- Custom exception nếu event name invalid.

Public API gợi ý:

```java
EventEmitter emitter = new EventEmitter();
Subscription sub = emitter.on("user.created", event -> {
    System.out.println(event.payload());
});
emitter.emit("user.created", new UserCreated("u1"));
sub.unsubscribe();
```

## Cấu Trúc Project Gợi Ý

```text
src/
  main/
    java/
      dev/thinh/javacore/
  test/
    java/
      dev/thinh/javacore/
```

README cần có:

- Thư viện này làm gì.
- Cách dùng API public.
- Ví dụ code ngắn.
- Những scope chưa làm.
- Tradeoff thiết kế.

## Checklist Capstone

- [ ] Có Maven project.
- [ ] Có package name rõ.
- [ ] Public API nhỏ và dễ đọc.
- [ ] Có validation input.
- [ ] Có exception rõ nghĩa.
- [ ] Có unit test happy path.
- [ ] Có unit test edge case.
- [ ] Có `README.md`.
- [ ] Có learning log giải thích 3 quyết định thiết kế.
