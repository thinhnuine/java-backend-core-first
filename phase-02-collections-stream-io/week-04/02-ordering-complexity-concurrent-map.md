# Ordering, Complexity Và Concurrent Collections

## `TreeMap`

`TreeMap` dựa trên balanced tree.

- `get`, `put`, `remove`: O(log n).
- Key được sort theo natural order hoặc `Comparator`.
- Phù hợp khi cần sorted key hoặc range query.

Ví dụ:

```java
Map<LocalDate, List<Task>> tasksByDate = new TreeMap<>();
```

## `TreeSet`

`TreeSet` tương tự `TreeMap` nhưng lưu value unique.

Cần chú ý `Comparator` phải nhất quán với equality theo cách bạn mong muốn.

```java
Set<String> names = new TreeSet<>(String.CASE_INSENSITIVE_ORDER);
```

Case này `"java"` và `"JAVA"` bị coi là trùng theo comparator.

## Big-O Cần Nhớ

| Operation | ArrayList | HashMap/HashSet | TreeMap/TreeSet |
| --- | --- | --- | --- |
| lookup by index | O(1) | n/a | n/a |
| contains value | O(n) | O(1) avg by key | O(log n) |
| add | O(1) amortized | O(1) avg | O(log n) |
| remove | O(n) by value/index shift | O(1) avg | O(log n) |
| sorted iteration | no | no | yes |

## `ConcurrentHashMap`

`ConcurrentHashMap` dùng cho concurrent access.

Nó không biến mọi flow thành thread-safe. Operation đơn lẻ an toàn, nhưng logic nhiều bước có thể vẫn race.

Sai:

```java
Integer current = counts.get(key);
counts.put(key, current == null ? 1 : current + 1);
```

Tốt hơn:

```java
counts.merge(key, 1, Integer::sum);
```

Hoặc:

```java
cache.computeIfAbsent(key, ignored -> loadValue(key));
```

## Khi Nào Dùng Concurrent Collection

Dùng khi:

- Nhiều thread cùng đọc/ghi.
- Bạn hiểu operation nào atomic.
- Bạn chấp nhận model consistency của collection đó.

Không dùng để:

- Né thiết kế ownership state.
- Thay transaction.
- Bảo vệ invariant nhiều object mà không có lock/coordination.

## Bài Tập Nhanh

Viết `WordCounter`:

- Single-thread version dùng `HashMap`.
- Multi-thread version dùng `ConcurrentHashMap.merge`.
- Tạo input nhiều dòng text.
- So sánh code clarity và correctness.
