# Bài Tập Tuần 11

## Bài 1: Test LRU Cache

Test:

- put/get.
- key không tồn tại.
- eviction order.
- get làm item trở thành recently used.
- capacity invalid.

## Bài 2: Test Event Emitter

Test:

- listener được gọi theo thứ tự đăng ký.
- unsubscribe xong không gọi nữa.
- emit event không có listener không lỗi.
- listener throw exception thì policy là gì.

## Bài 3: Parameterized Validation

Chọn class có validation.

Viết parameterized test cho:

- null.
- blank.
- negative number.
- invalid format.

## Bài 4: Mock/Fake Comparison

Với một service phụ thuộc interface:

- viết test bằng Mockito.
- viết test bằng fake.
- ghi nhận bản nào rõ hơn và vì sao.

## Learning Log

Trả lời:

- Test nào bắt được bug thật?
- Test nào quá phụ thuộc implementation?
- Bạn có mock quá tay không?
- Assertion fail có dễ hiểu không?

Theo lịch mới, ưu tiên test/refactor TaskService và TaskRepository của Task Manager đang có. LRU, parser, event emitter và log processor chỉ là lựa chọn nếu bạn đã làm các project đó; không cần tạo thêm project để học testing/design.
