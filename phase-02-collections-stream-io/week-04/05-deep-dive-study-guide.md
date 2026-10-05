# Deep Dive Tuần 4: Collections Internals

## Cách Học Tuần Này

Collections là nền rất quan trọng vì backend gần như lúc nào cũng biến đổi dữ liệu: list user, map config, set permission, group log, cache result. Đừng chỉ học thuộc Big-O. Hãy học cách chọn collection theo câu hỏi: cần lookup nhanh, giữ thứ tự, sort, unique, hay thread-safe?

## ArrayList

`ArrayList` giống mảng có thể tự resize.

Mạnh ở:

- đọc theo index nhanh
- iterate nhanh
- thêm cuối thường nhanh

Yếu ở:

- insert đầu list
- remove giữa list
- `contains` trên list lớn

Mental model:

```text
[a][b][c][ ][ ][ ]
```

Khi đầy, nó tạo array lớn hơn rồi copy dữ liệu sang.

## HashMap

`HashMap` dùng hash của key để tìm bucket. Muốn `HashMap` đúng, key phải có `equals/hashCode` đúng và ổn định.

Key mutable là nguồn bug rất đau:

```java
map.put(userKey, value);
userKey.changeEmail("new@example.com");
map.get(userKey); // có thể không tìm thấy
```

Vì hash bucket ban đầu dựa trên email cũ.

## HashSet

`HashSet` gần như `HashMap` chỉ dùng key. Nó hợp cho membership:

```java
if (blockedUserIds.contains(userId)) {
}
```

Nếu bạn đang dùng `List.contains` trên list lớn chỉ để check tồn tại, hãy nghĩ tới `HashSet`.

## TreeMap Và TreeSet

`TreeMap` giữ key sorted. Đổi lại operation là O(log n), không phải O(1) trung bình như `HashMap`.

Dùng khi:

- cần sorted iteration
- cần range query
- cần floor/ceiling key

Không dùng chỉ vì "nghe có thứ tự". Nếu chỉ cần sort lúc output, có thể dùng `HashMap` rồi sort sau.

## LinkedHashMap

`LinkedHashMap` giữ insertion order hoặc access order. Nó là công cụ tốt để hiểu LRU cache.

Nếu dùng access order:

```java
new LinkedHashMap<>(16, 0.75f, true)
```

mỗi lần `get`, entry được xem là vừa dùng.

## ConcurrentHashMap

`ConcurrentHashMap` thread-safe cho operation đơn lẻ. Nhưng logic nhiều bước vẫn có thể race.

Sai:

```java
if (!map.containsKey(key)) {
    map.put(key, load(key));
}
```

Tốt hơn:

```java
map.computeIfAbsent(key, this::load);
```

## Cách Chọn Collection

Hỏi theo thứ tự:

- Có cần duplicate không? Không thì `Set`.
- Có cần lookup by key không? Có thì `Map`.
- Có cần giữ thứ tự insert không? `LinkedHashMap` hoặc `ArrayList`.
- Có cần sorted không? `TreeMap`/`TreeSet` hoặc sort lúc output.
- Có nhiều thread ghi không? Nghĩ tới concurrent structure hoặc lock.

## Bài Tập Theo Bước

1. Tạo list 100k integer.
2. So sánh `ArrayList.contains` và `HashSet.contains`.
3. Tạo key class đúng `equals/hashCode`.
4. Tạo key mutable để thấy bug.
5. Viết note chọn collection cho 6 tình huống thực tế.

## Lỗi Thường Gặp

- Dùng `List` cho lookup lặp lại nhiều lần.
- Dùng `HashMap` nhưng key mutable.
- Nghĩ `ConcurrentHashMap` giải quyết mọi vấn đề concurrency.
- Chọn `TreeMap` khi không cần sorted behavior.

## Câu Hỏi Tự Kiểm Tra

- Vì sao `ArrayList.get(index)` nhanh?
- Vì sao `HashMap` cần `hashCode`?
- Khi nào `TreeMap` đáng dùng?
- `LinkedHashMap` giúp gì cho LRU?
- Operation đơn lẻ thread-safe khác gì flow thread-safe?
