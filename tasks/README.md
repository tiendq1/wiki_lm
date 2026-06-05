# Tasks README

File này là hướng dẫn và template mẫu cho task.

Lưu ý: bản thân file này không phải là task.

Chỉ tạo task trong `tasks/` khi người dùng nói rõ đó là task, hoặc yêu cầu "ghi task", "tạo task", "lưu task".

## Folder task

- `tasks/mobile/`: task liên quan mobile, React Native, iOS/Android.
- `tasks/web/`: task liên quan web, React, UI/CSS.

## Template task

```md
# Task: <tên task>

- Ngày tạo:
- Nguồn:
- Project:
- Project path:
- Loại: mobile | web | docs | research | personal | other
- Trạng thái: todo | in-progress | completed | partial | blocked

## Mục tiêu

- ...

## Input

- Tài liệu:
  - ...
- File/source liên quan:
  - ...
- Thông tin người dùng cung cấp:
  - ...

## Output mong muốn

- ...

## Phạm vi

### Làm

- ...

### Không làm

- ...

## Ràng buộc / lưu ý

- ...

## Cách kiểm tra

- ...

## Kết quả sau khi làm

- ...

## File/thư mục đã thay đổi

- ...

## Ghi chú cho agent

- ...
```

## Rule sau khi xử lý task

- Nếu là bug fix, ghi lỗi vào `errors/` và command quan trọng vào `terminal/fixbug.md`.
- Nếu là task mobile, ưu tiên đọc `WIKI.md`, `memory/current-context.md`, `profile/current-projects.md`, và `projects/<project-name>/`.
- Sau khi task hoàn thành, cập nhật `daily-report/YYYY-MM-DD.md`.
- Nếu task tạo ra bài học dùng lại được, cập nhật `memory/lessons-learned.md`.
- Nếu task làm thay đổi hiểu biết ổn định về người dùng/project, cập nhật `profile/`, `projects/`, hoặc `memory/` tương ứng.
