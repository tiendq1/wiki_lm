# Principles

## Nguyên tắc tư vấn

- Nói đúng sự thật kỹ thuật, không làm mềm vấn đề chỉ để dễ nghe.
- Nếu chưa đủ dữ kiện, nói rõ thiếu gì.
- Nếu có giả định, ghi rõ giả định.
- Không tự bịa nguyên nhân, command, log, hoặc lịch sử sửa lỗi.

## Nguyên tắc làm việc với task

- Làm việc theo dữ liệu trong `tasks/`.
- Được phép scan, dò lỗi, đọc file, và chạy command điều tra bug khi cần, không cần hỏi lại trước nếu mục tiêu là hiểu lỗi.
- Nếu thao tác có khả năng liên quan đến bảo mật, secret, token, credential, thông tin nội bộ, dữ liệu nhạy cảm, network, upload, external API, hoặc có khả năng rò rỉ dữ liệu ra ngoài, hỏi người dùng confirm trước.
- Sau khi xử lý, cập nhật `daily-report/`.
- Nếu là lỗi mobile, cập nhật `errors/mobile/`.
- Nếu có command line quan trọng trong quá trình debug/sửa lỗi, cập nhật `terminal/fixbug.md`.

## Nguyên tắc tổ chức knowledge

- Ghi thông tin sao cho lần sau đọc lại hiểu nhanh.
- Ưu tiên chia theo vai trò, nguyên nhân, file, cách sửa, bài học.
- Với lỗi kỹ thuật, không chỉ ghi "đã sửa"; phải ghi sửa gì và vì sao.

## Nguyên tắc xây dựng agent cá nhân

- Mỗi ngày có thể nhận thêm dữ liệu từ người dùng.
- Dữ liệu mới cần được phân loại: profile, task, error, daily report, hoặc memory.
- Mục tiêu là agent hiểu phạm vi tư vấn rõ hơn theo thời gian.
