# Deep Dive Tuần 12: SOLID, Pattern Và Refactoring

## Cách Học Tuần Này

Đừng học pattern như bộ sưu tập tên gọi. Học theo câu hỏi: code đang đau ở đâu, pattern nào giảm đau, và tradeoff là gì?

## SOLID Thực Dụng

Single Responsibility không có nghĩa là mỗi class chỉ có một method. Nó nghĩa là class có một lý do chính để thay đổi.

Open/Closed không có nghĩa là không bao giờ sửa code cũ. Nó nghĩa là thiết kế cho phép thêm behavior mới mà không phá nhiều code ổn định.

Dependency Inversion không có nghĩa là mọi class đều cần interface. Nó nghĩa là high-level policy không bị trói vào low-level detail.

## Khi Nào Refactor

Refactor khi có tín hiệu:

- test khó viết
- method quá dài
- class biết quá nhiều
- thêm feature phải sửa nhiều chỗ
- bug lặp lại quanh cùng vùng code
- duplication mang ý nghĩa business

Không refactor chỉ vì "cho sạch" nếu bạn không mô tả được vấn đề.

## Builder

Builder hợp khi object có nhiều optional field hoặc constructor quá dài.

Không cần builder cho:

```java
new Money(100, "USD")
```

Cần cân nhắc builder cho:

```java
UserProfile.builder()
    .id("u1")
    .email("a@example.com")
    .displayName("Thinh")
    .timezone("Asia/Ho_Chi_Minh")
    .build();
```

## Strategy

Strategy hợp khi bạn có nhiều thuật toán thay thế nhau.

Ví dụ:

- discount policy
- retry policy
- pricing rule
- rate limiter algorithm

Nếu bạn thấy `switch`/`if` dài theo type thuật toán, nghĩ tới Strategy.

## Factory

Factory hợp khi logic tạo object phức tạp hoặc bạn muốn ẩn concrete class.

Nhưng factory quá sớm có thể làm code vòng vèo. Nếu `new` rõ ràng và ổn, cứ `new`.

## Observer

Observer hợp cho event/listener. Event emitter của bạn là bài tập tự nhiên.

Câu hỏi quan trọng:

- Listener chạy theo thứ tự nào?
- Listener lỗi thì emit có dừng không?
- Unsubscribe có thật sự remove không?

## Refactoring Workflow

1. Viết test bảo vệ behavior.
2. Chọn một code smell cụ thể.
3. Sửa nhỏ.
4. Chạy test.
5. Commit.
6. Lặp lại.

## Bài Tập Theo Bước

1. Chọn một bài cũ.
2. Viết `refactoring-notes.md`.
3. Liệt kê 2 code smell.
4. Viết/bổ sung test.
5. Refactor một phần.
6. Chạy test.
7. Ghi tradeoff.

## Lỗi Thường Gặp

- Pattern hóa quá tay.
- Tạo interface cho mọi class.
- Refactor không có test.
- Đổi public API mà không ghi lý do.
- Tách class quá nhỏ làm flow khó đọc.

## Câu Hỏi Tự Kiểm Tra

- Vấn đề cụ thể trước refactor là gì?
- Pattern này giảm complexity hay tăng ceremony?
- Test có bảo vệ behavior quan trọng chưa?
- Public API sau refactor dễ dùng hơn không?
- Tradeoff bạn chấp nhận là gì?
