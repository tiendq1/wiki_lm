# Memory

Thư mục này lưu phần tóm tắt đã được chưng cất từ profile, daily report, task, error, và project docs.

Mục tiêu: giúp agent đọc nhanh context ổn định thay vì phải đọc toàn bộ repo mỗi lần.

## File chính

- `stable-memory.md`: thông tin ổn định về người dùng và cách làm việc.
- `current-context.md`: ưu tiên hiện tại, project đang chú ý, trạng thái gần nhất.
- `lessons-learned.md`: kinh nghiệm kỹ thuật/bài học từ lỗi và task đã làm.

## Cách cập nhật

- Chỉ đưa thông tin đã rõ hoặc đã được người dùng xác nhận vào memory.
- Nếu thông tin chỉ đúng trong ngày, ghi vào `daily-report/` trước.
- Nếu một pattern lặp lại hoặc có giá trị tái sử dụng, đưa vào `lessons-learned.md`.
- Nếu ưu tiên project thay đổi, cập nhật `current-context.md`.
