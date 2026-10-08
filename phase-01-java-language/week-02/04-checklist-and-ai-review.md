# Checklist Cơ Bản: Generics Và Enum

Chỉ đánh dấu khi đã tự chạy code để chứng minh. Bounded type, wildcard/PECS, erasure và LRU không phải điều kiện qua lượt học đầu.

- [ ] Đọc được List<String> và Box<TaskTitle> bằng lời của mình.
- [ ] Viết được Box<T> có constructor/get và sử dụng với hai kiểu khác nhau.
- [ ] Thử sai kiểu và thấy lỗi compile; phân biệt với ClassCastException lúc chạy.
- [ ] Biết vì sao dùng Integer thay vì int làm type argument.
- [ ] Khai báo và truyền TaskStatus qua method, viết được label.
- [ ] Ghép List<Task> với enum để in hai task.
- [ ] Có test label cho status hợp lệ và null sau khi học JUnit.

## Prompt review

```text
Tôi có 4 năm làm FE nhưng mới học Java. Tôi vừa học List<T>, Box<T> và enum.
Đây là code, kết quả tôi dự đoán, và kết quả thực tế.
Hãy chọn tối đa 2 vấn đề quan trọng. Với mỗi vấn đề:
1. Chỉ ra dòng liên quan và hỏi tôi dự đoán kết quả.
2. Giải thích bằng một ví dụ nhỏ, chưa dùng wildcard/PECS hoặc design pattern.
3. Cho một gợi ý sửa, chưa viết toàn bộ lời giải.
Nếu bài đã ổn, đưa một biến thể nhỏ để tôi tự làm.
```

Tiếp theo: [module record và exception](../week-03/README.md). Khi cần tự thiết kế API generic, quay lại [tra cứu nâng cao](06-advanced-reference.md).
