# Mentor Explanation Style

Quy ước biên tập để các lần sửa sau giữ cùng cách dạy. Nội dung ưu tiên yêu cầu mới nhất của người học.

## Required Structure For Each Concept

Viết cho FE có 4 năm React/Next.js nhưng mới học Java. Giữ heading cấp 2 của file hiện có; nếu cần chia nhỏ thì thêm heading cấp 3. Mỗi chủ đề chỉ tập trung một việc.

Thứ tự: một câu định nghĩa và mục đích → code Java 21 tối đa 10 dòng, comment dòng quan trọng → liên hệ FE nếu đúng → “Khi nào dùng” 2–3 ý → “Lỗi hay gặp” tối đa 3 ý → một quy tắc dễ nhớ → bài tập nhỏ và gợi ý, không đáp án.

Với file cũ có tám heading về cách học một chủ đề, giữ tên/thứ tự để đối chiếu nhưng rút phần nội dung xuống ngắn; không lặp tám mục đó cho mỗi tiểu mục. README, đề bài và checklist phục vụ điều hướng/thực hành, không thêm code vô nghĩa chỉ để đủ khuôn.

## Tone

Dùng tiếng Việt đời thường. Giữ thuật ngữ tiếng Anh và giải thích ngay lần đầu. Không giả định đã biết SQL, backend Node.js, Docker hay bảo mật. Mỗi phần nên vừa một lượt đọc ngắn; tách nếu cần cuộn nhiều. Không ép so sánh với React khi khác cơ chế.

## Short Answer Exception

Câu hỏi nhanh trả lời ngắn đúng nhu cầu. Khi xin gợi ý bài tập, chỉ gợi ý bước tiếp theo; không đưa toàn bộ lời giải. Khi sửa tài liệu, nêu trước điểm sai hoặc dễ hiểu nhầm được phát hiện.

## Learning Path And Prerequisites

Theo ROADMAP 24 tuần; week trong đường dẫn chỉ là mã module cũ. Ví dụ chính dùng Java 21 thuần, có cách chạy rõ: JShell độc lập hoặc file có main. Không nén nhiều câu lệnh khó đọc để lách giới hạn 10 dòng. Khi cần nhiều file, chia từng bước và ghi tên file.

Giữ chi tiết nâng cao ở “Đọc thêm sau” cuối file. Mô hình Java thuần về Spring/DB/security phải ghi rõ chưa thực hiện framework, lưu trữ hay xác thực thật. Bài SQL chạy trong SQL console; shell command chạy trong terminal, không giả làm Java.

## Course Outcomes And Evidence

Đạt bài khi tự thực hiện, giải thích và kiểm một trường hợp lỗi. Một tuần mới có tổng 8 giờ cho nội dung được chọn. Task Manager xuyên suốt; parser/cache/thread pool là tự chọn.

Chỉ báo kiểm chứng đã làm thật: chạy snippet không chứng minh API/DB/security/container. Race demo đúng vài lần không chứng minh đồng bộ đúng. Không đánh đổi việc giải thích chính xác để có câu ngắn dễ nhớ.

## Đọc thêm sau

Internals (cách triển khai bên trong), tuning (điều chỉnh hiệu năng), ngoại lệ hiếm và chi tiết phụ thuộc phiên bản chỉ đưa vào khi cần. Tài liệu chính thức bổ trợ phần nâng cao; phần cơ bản cần đủ để người học chạy ví dụ tại chỗ.
