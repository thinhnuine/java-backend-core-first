# Project Tuần 12: Refactor Một Bài Cũ

## Chọn Một Project

- LRU cache.
- Event emitter.
- Log processor.
- JSON parser.

## Yêu Cầu

- Viết test trước khi refactor.
- Tách class đang làm quá nhiều việc.
- Áp dụng 1 pattern nếu có lý do rõ.
- Không đổi public API nếu không cần.
- Nếu đổi public API, ghi migration note.

## `refactoring-notes.md`

Nội dung:

- Vấn đề ban đầu.
- Smell quan sát được.
- Thay đổi đã làm.
- Pattern dùng, nếu có.
- Tradeoff.
- Test bảo vệ behavior nào.
- Điều chưa sửa.

## Gợi Ý Theo Project

LRU cache:

- Tách storage và eviction policy nếu implementation phình.
- Strategy cho eviction nếu muốn thử.

Event emitter:

- Observer pattern tự nhiên.
- Chú ý unsubscribe và exception policy.

Log processor:

- Tách parser, aggregator, reporter.
- Strategy cho output format nếu cần.

JSON parser:

- Tách tokenizer/parser/model.
- Sealed type cho JSON AST.

## Acceptance

- Test pass.
- Code dễ đọc hơn theo note.
- Không thêm abstraction vô cớ.
- Public API vẫn dễ dùng.
