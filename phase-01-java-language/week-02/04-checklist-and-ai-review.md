# Checklist Và AI Review Tuần 2

## Checklist

- [ ] Viết được generic class.
- [ ] Viết được generic method.
- [ ] Hiểu bounded type như `<T extends Comparable<T>>`.
- [ ] Giải thích được PECS bằng ví dụ.
- [ ] Biết vì sao không thể `new T()`.
- [ ] Dùng được `Optional<T>` cho case có thể không tìm thấy.
- [ ] Dùng enum có field/method.
- [ ] Biết khi nào dùng static nested class.
- [ ] Có test cho generic repository hoặc LRU cache.

## Prompt AI Review

```text
Bạn là mentor Java Backend. Hãy review code Java generics sau.

Tập trung vào:
- generic type parameter có rõ nghĩa không
- wildcard có cần thiết không
- có vi phạm PECS không
- có expose mutable collection không
- Optional dùng hợp lý không
- enum có đang chứa behavior đúng chỗ không
- test có cover edge case chưa

Đừng rewrite toàn bộ. Hãy chỉ ra vấn đề, giải thích rủi ro và đề xuất chỉnh sửa nhỏ.

Code:
```

## Câu Hỏi Tự Vấn

- API generic có làm caller dễ dùng hơn không?
- Có đang thêm generic chỉ để code trông "xịn" hơn không?
- `findAll` có trả internal list/map ra ngoài không?
- LRU cache có test eviction order chưa?
- Enum có giúp xóa `if/else` rải rác không?
