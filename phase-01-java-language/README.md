# Giai Đoạn 1: Ngôn Ngữ Java (Tuần 1-3)

## Mục Tiêu

Sau 3 tuần, bạn nên viết được một thư viện Java nhỏ có API rõ, có unit test cơ bản, không copy implementation từ AI, và giải thích được các quyết định thiết kế chính.

Giai đoạn này tập trung vào:

- Type system và khác biệt Java vs TypeScript.
- OOP kiểu Java: interface, abstract class, inheritance, composition.
- Object contract: `equals`, `hashCode`, `toString`.
- Immutability.
- Generics, wildcard, type erasure.
- `enum`, nested/inner/anonymous class.
- Java 17/21: record, sealed class, pattern matching, switch expression, text block.
- Exception: checked vs unchecked, try-with-resources.
- Maven project structure.

## Nhịp 8h/tuần

- 2h học concept và đọc ví dụ.
- 4h tự code bài tập.
- 1h viết test, debug, refactor.
- 1h AI review và ghi learning log.

## Deliverable Cuối Giai Đoạn

Chọn một thư viện nhỏ để port từ JS/Python sang Java:

- JSON parser đơn giản.
- LRU cache.
- Event emitter.

Yêu cầu:

- Maven project chạy được.
- API public rõ ràng.
- Unit test cho happy path và edge case.
- README giải thích cách dùng.
- Ghi chú tradeoff: vì sao chọn interface/abstract class/composition/generic/exception như vậy.

## Gợi Ý Setup

Nếu đã có Java 21 và Maven:

```bash
java --version
mvn --version
```

Tạo project luyện tập:

```bash
mvn archetype:generate \
  -DgroupId=dev.thinh.javacore \
  -DartifactId=java-core-lab \
  -DarchetypeArtifactId=maven-archetype-quickstart \
  -DinteractiveMode=false
```

Nếu máy chỉ có Java 17 thì vẫn học được phần lớn nội dung. Riêng virtual threads thuộc giai đoạn sau mới cần Java 21 rõ hơn.
