# Checklist Và AI Review Tuần 10

## Checklist

- [ ] Dùng được `jcmd` để xem JVM process.
- [ ] Lấy được heap dump.
- [ ] Giải thích được leak là object còn reachable.
- [ ] Viết được custom annotation runtime.
- [ ] Dùng reflection đọc annotation/method.
- [ ] Viết được dynamic proxy đơn giản.
- [ ] Proxy không làm mất root cause khi method lỗi.
- [ ] Liên hệ được proxy với Spring transaction/security.

## Prompt AI Review

```text
Bạn là mentor Java Backend. Hãy review bài JVM tooling/reflection/proxy của tôi.

Tập trung vào:
- phân tích memory leak có hợp lý không
- heap dump observation có đủ bằng chứng không
- annotation retention/target có đúng không
- reflection code có xử lý exception ổn không
- dynamic proxy có giữ được cause khi method lỗi không
- liên hệ với Spring có đúng bản chất không

Code và ghi chú:
```
