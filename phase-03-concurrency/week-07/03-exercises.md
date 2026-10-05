# Bài Tập Tuần 7

## Bài 1: Async User Profile

Viết `AsyncUserProfileService`.

Fake dependency:

- `UserClient#getUser(userId)`.
- `OrderClient#getOrders(userId)`.

Yêu cầu:

- Gọi user và order song song bằng `CompletableFuture`.
- Combine thành `UserProfile`.
- Có timeout hoặc error handling rõ.
- Không dùng common pool nếu muốn kiểm soát executor.

## Bài 2: Concurrent Word Counter

Yêu cầu:

- Nhận list dòng text.
- Chia cho nhiều task xử lý.
- Dùng `ConcurrentHashMap<String, Integer>` hoặc `LongAdder`.
- Dùng `merge`, `compute` hoặc atomic update đúng.

## Bài 3: Deadlock Lab

Tạo deadlock có chủ đích:

- 2 lock.
- 2 thread acquire ngược thứ tự.

Sau đó sửa:

- lock ordering.
- hoặc `tryLock` timeout.

## Bài 4: Atomic Counter

So sánh:

- `synchronized`.
- `AtomicInteger`.
- `LongAdder` nếu muốn đọc thêm.

Ghi nhận:

- code clarity.
- performance cảm tính có đo nhẹ.
- tradeoff.
