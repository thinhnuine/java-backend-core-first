# Checklist Và AI Review Tuần 4

Chỉ tự kiểm phần **làm quen List, Set, Map**. Chưa đánh giá kiến thức sâu.

## Checklist

- [ ] Tự tạo được List, thêm phần tử và đọc theo vị trí.
- [ ] Biết Set bỏ phần tử trùng.
- [ ] Dùng Map lưu/đọc/xóa theo ID.
- [ ] Hiểu put lại cùng key và get khi thiếu key.
- [ ] Tự đổi input và giải thích output.

## Prompt AI Review

```text
Tôi đang làm quen Java, đã biết React/Next.js.
Review thao tác List/Set/Map trong code dưới đây.
Chỉ ra tối đa 2 lỗi hoặc chỗ khó hiểu, giải thích từ mới ngay khi dùng.
Cho gợi ý để tôi tự sửa; chưa mở rộng sang internals hoặc tối ưu hiệu năng.
Code, dự đoán và kết quả thực tế:
```

## Câu Hỏi Tự Vấn

- Dòng này thêm, đọc hay xóa dữ liệu?
- Đổi key hoặc thêm phần tử trùng thì kết quả ra sao?
- Tôi chọn List/Set/Map vì cần thao tác nào?

Đủ khi tự trả lời bằng ví dụ đã chạy. Nếu còn vướng, quay lại bài 00.

## Đọc thêm sau

- Checklist sâu: Big-O (mức tăng chi phí theo kích thước dữ liệu), equals/hashCode, sorted order và insertion order.
- Review sâu: benchmark công bằng, kết luận theo bằng chứng, key ổn định, atomic operation của ConcurrentHashMap.
- Câu tự vấn sau này: dữ liệu 1k → 1M, key immutable, sort liên tục hay khi output, lỗi nhiều thread được xử lý đúng chưa?
