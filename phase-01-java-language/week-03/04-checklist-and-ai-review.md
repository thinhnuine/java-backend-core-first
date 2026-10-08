# Checklist Cơ Bản: Record, Switch Và Exception

- [ ] Tạo được record, gọi accessor và nêu những method Java sinh hộ.
- [ ] Biết record không tự validate null, không tự làm mọi object bên trong immutable.
- [ ] Viết được switch expression trên enum ba giá trị; thử lỗi thiếu case.
- [ ] Dự đoán đúng luồng chạy của try/throw/catch với input hợp lệ và sai.
- [ ] Phân biệt throw, throws và catch bằng code đã chạy.
- [ ] Giải thích checked exception yêu cầu catch/declare, nhưng vẫn phát sinh khi chạy.
- [ ] Hoàn thành [bài summary](03-task-practice.md) và test các case biên.

Sealed type, pattern matching, custom exception và capstone thư viện chưa phải điều kiện qua bài. Try-with-resources học khi làm I/O/JDBC.

## Prompt review

```text
Tôi là FE 4 năm, mới học Java. Bài hiện tại chỉ dùng record, enum,
switch expression và exception cơ bản. Đây là code và test tôi tự viết.
Hãy kiểm tra validation, luồng throw/catch, chỗ tôi hiểu sai record và case test còn thiếu.
Chỉ chọn tối đa 2 vấn đề mỗi lượt, giải thích theo input → dòng code → output.
Chưa thêm sealed class, generic result, framework hoặc viết lại toàn bộ bài.
Hỏi tôi một câu kiểm tra hiểu sau mỗi gợi ý.
```

Tiếp theo: quay về [roadmap](../../ROADMAP.md). Nếu chưa giải thích được một mục, chạy lại đúng ví dụ của mục đó trước khi mở [tra cứu nâng cao](06-advanced-reference.md).
