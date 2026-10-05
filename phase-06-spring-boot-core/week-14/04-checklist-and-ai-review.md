# Checklist Và AI Review Tuần 14

## Checklist

- [ ] Có Flyway migration.
- [ ] Entity không expose API lung tung.
- [ ] Transaction nằm ở service layer.
- [ ] Không trả entity trực tiếp nếu DTO phù hợp hơn.
- [ ] Có xử lý not found rõ.
- [ ] Biết query nào có nguy cơ N+1.
- [ ] Có integration test flow chính.
- [ ] README đủ để chạy lại project.

## Prompt AI Review

```text
Bạn là mentor Java Spring Boot. Hãy review final API project của tôi.

Tập trung vào:
- entity relationship có hợp lý không
- transaction boundary có đúng không
- có nguy cơ N+1 không
- DTO/entity mapping có rõ không
- validation và error response có nhất quán không
- integration test có cover flow chính không
- README có đủ để người khác chạy không
- tôi có hiểu đúng annotation Spring/JPA đang dùng không

Code, schema và README:
```
