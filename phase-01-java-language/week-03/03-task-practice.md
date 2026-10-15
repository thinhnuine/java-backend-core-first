# Thực Hành Nhỏ: Tóm Tắt Task Và Báo Input Sai

Đây là đề để tự làm, không phải đáp án. Chọn từng mục theo README module; không cần làm hết trong một buổi. Tự gõ code, dùng AI để xin gợi ý sau khi đã thử.

## Bước 1 — Mang dữ liệu

Record mang dữ liệu của kết quả tóm tắt.

- Tạo TaskStatus với ba giá trị.
- Tạo TaskSummary chứa id, title, status.
- In các method đọc.

**Gợi ý:** Record không tự chặn dữ liệu sai.

**Đạt khi:** Tạo được hai record và so equals. Tự gõ phần triển khai, không dùng AI sinh lời giải.

## Bước 2 — Viết method tạo summary

Method createSummary kiểm input trước khi trả kết quả.

- id dương, title khác null/trắng và dài 1–100 sau trim.
- status khác null.
- Sai thì throw IllegalArgumentException.

**Gợi ý:** Gọi constructor record trực tiếp vẫn có thể bỏ qua method này; đó là giới hạn bài.

**Đạt khi:** Một input hợp lệ trả title đã trim. Tự gõ phần triển khai, không dùng AI sinh lời giải.

## Bước 3 — Chạy và quan sát lỗi

Caller là bên gọi method và nhận kết quả hoặc lỗi.

- Viết lần gọi hợp lệ.
- Viết lần gọi sai trong try/catch.
- In message của lỗi.

**Gợi ý:** Không catch rồi trả null trong createSummary.

**Đạt khi:** Biết dòng nào không được chạy sau throw. Tự gõ phần triển khai, không dùng AI sinh lời giải.

## Bước 4 — Tự viết test

Test kiểm các trường hợp sát quy tắc.

- Thử id 0, title null/trắng, status null.
- Thử title 100 và 101 ký tự sau trim.

**Gợi ý:** Xác định expected trước khi chạy.

**Đạt khi:** Mỗi case đúng/lỗi đều được kiểm, không chỉ quan sát bằng mắt. Tự gõ phần triển khai, không dùng AI sinh lời giải.

## Đạt khi

Tự thêm một quy tắc mới và kiểm nó. **Gợi ý:** nếu chưa giải thích được throw/throws, quay lại [bài exception](02-exception-handling.md). Chưa cần parser hay sealed class.

## Đọc thêm sau

Chỉ mở rộng sau khi bài nhỏ chạy đúng. Ghi một lỗi đã gặp và cách bạn tự tìm nguyên nhân.
