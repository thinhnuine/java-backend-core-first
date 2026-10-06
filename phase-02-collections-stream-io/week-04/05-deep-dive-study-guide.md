# Mentor Guide Tuần 4: Collections Internals

## 1. Ý chính

Collections là bộ công cụ lưu và tìm dữ liệu trong Java. Backend code hầu như lúc nào cũng dùng collection: list task, map config, set permission, cache response. Học phần này để chọn đúng cấu trúc dữ liệu thay vì dùng `ArrayList` và `HashMap` theo thói quen.

## 2. Giải thích code

Ví dụ so sánh lookup:

```java
package dev.thinh.javacore;

import java.util.ArrayList;
import java.util.HashSet;
import java.util.List;
import java.util.Set;

public class CollectionLookupDemo {
    public static void main(String[] args) {
        List<Integer> list = new ArrayList<>();
        Set<Integer> set = new HashSet<>();

        for (int i = 0; i < 100_000; i++) {
            list.add(i);
            set.add(i);
        }

        System.out.println(list.contains(99_999));
        System.out.println(set.contains(99_999));
    }
}
```

Giải thích:

- `List<Integer>`: danh sách có thứ tự, cho phép duplicate.
- `ArrayList`: implementation dựa trên array.
- `Set<Integer>`: tập unique, không quan tâm duplicate.
- `HashSet`: implementation dựa trên hash table.
- `list.contains(...)`: phải duyệt tuần tự, thường O(n).
- `set.contains(...)`: dùng hash, trung bình O(1).

Ví dụ `HashMap` key:

```java
Map<UserId, String> names = new HashMap<>();
names.put(new UserId("u1"), "Thinh");
System.out.println(names.get(new UserId("u1")));
```

Code này chỉ hoạt động đúng nếu `UserId` có `equals/hashCode` đúng.

## 3. Vì sao thiết kế như vậy

Mỗi collection tối ưu cho một kiểu thao tác.

Nếu bạn dùng `List` để check membership lặp đi lặp lại:

```java
for (User user : users) {
    if (blockedIdsList.contains(user.id())) {
        // ...
    }
}
```

Nếu `blockedIdsList` lớn, mỗi `contains` lại quét list. Dùng `HashSet` hợp hơn:

```java
Set<UserId> blockedIds = new HashSet<>(blockedIdsList);
```

Nếu key mutable, `HashMap` có thể hỏng:

```java
MutableUserKey key = new MutableUserKey("a@example.com");
map.put(key, "A");
key.changeEmail("b@example.com");
System.out.println(map.get(key)); // có thể null
```

Vì hash bucket ban đầu dựa trên email cũ.

## 4. Liên hệ với Frontend

JavaScript cũng có `Array`, `Map`, `Set`:

```javascript
const ids = new Set(["u1", "u2"])
ids.has("u1")
```

Java tương tự về ý tưởng, nhưng strict hơn về type:

```java
Set<String> ids = new HashSet<>();
```

Bạn phải chọn type của item ngay từ đầu.

## 5. Khi nào dùng và không dùng

| Collection | Khi dùng | Khi tránh |
| --- | --- | --- |
| `ArrayList` | Danh sách có thứ tự, đọc/iterate nhiều | Lookup membership nhiều lần |
| `HashSet` | Check tồn tại, unique item | Cần giữ sorted order |
| `HashMap` | Lookup value theo key | Key mutable hoặc thiếu equals/hashCode |
| `TreeMap` | Cần key sorted/range query | Chỉ cần lookup nhanh |
| `LinkedHashMap` | Cần giữ insertion/access order | Không cần order |
| `ConcurrentHashMap` | Nhiều thread đọc/ghi | Muốn né thiết kế concurrency |

## 6. Bẫy hay gặp

- Dùng `ArrayList.contains` trong loop lớn.
- Dùng object mutable làm key trong `HashMap`.
- Override `equals` nhưng quên `hashCode`.
- Nghĩ `HashMap` giữ thứ tự.
- Dùng `ConcurrentHashMap` nhưng logic nhiều bước vẫn race.

## 7. Thuật ngữ mới

- `Big-O`: cách mô tả độ phức tạp khi input lớn.
- `lookup`: tìm dữ liệu.
- `hash`: số tính từ object để tìm bucket.
- `bucket`: vùng lưu item trong hash table.
- `mutable key`: key có thể đổi state sau khi đưa vào map.
- `iteration order`: thứ tự khi duyệt collection.

## 8. Bài tập nhỏ để tự gõ lại

Tự viết `CollectionLookupDemo`, rồi:

1. Tạo 100k số.
2. So sánh `ArrayList.contains` và `HashSet.contains`.
3. Tạo `UserId` đúng `equals/hashCode`.
4. Dùng `UserId` làm key trong `HashMap`.

Học tiếp theo: nếu thấy `HashMap` vẫn mơ hồ, quay lại bài `equals/hashCode` tuần 1 và thử phá nó bằng key mutable.
