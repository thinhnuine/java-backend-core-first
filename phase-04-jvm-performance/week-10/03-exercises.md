# Bài Tập Tuần 10

## Bài 1: Heap Dump Analysis

Tạo app leak có chủ đích:

- static list.
- cache không eviction.
- listener không unsubscribe.

Chọn ít nhất 1 kiểu.

Yêu cầu:

- Chạy app.
- Lấy heap dump.
- Tìm object bị giữ.
- Viết `memory-leak-analysis.md`.

## Bài 2: Mini Component Scanner

Yêu cầu:

- Annotation `@MyComponent`.
- Class `UserService`, `OrderService` có annotation.
- Class `PlainHelper` không có annotation.
- Method scanner nhận `List<Class<?>>`.
- Trả về list class có annotation.

Test:

- Scanner tìm đúng class.
- Scanner bỏ qua class không annotate.

## Bài 3: Dynamic Proxy Logger

Yêu cầu:

- Interface `Calculator`.
- Implementation `DefaultCalculator`.
- Proxy log method name, args, execution time.
- Nếu target throw exception, proxy không nuốt cause.

Test:

- Method success.
- Method throw exception.

## Bài 4: Spring Magic Note

Viết note:

- Spring biết class nào là bean bằng cách nào?
- Transaction proxy làm gì quanh method?
- Vì sao self-invocation có thể gây vấn đề với proxy?
