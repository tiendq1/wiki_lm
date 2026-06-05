# Task Template

Template này dùng khi người dùng nói rõ một yêu cầu là task.

Không tự tạo task cho các yêu cầu nạp data, bổ sung context, update profile, lưu tài liệu, hoặc chỉnh hệ thống WikiLLM, trừ khi người dùng nói rõ đó là task.

## Folder task

- `tasks/mobile/`: task liên quan mobile, React Native, iOS/Android.
- `tasks/web/`: task liên quan web, React, UI/CSS.

## Template

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
- Nếu là task mobile, ưu tiên kiểm tra `projects/`, `profile/current-projects.md`, và `memory/current-context.md`.
- Sau khi task hoàn thành, cập nhật `daily-report/YYYY-MM-DD.md`.
- Nếu task tạo ra bài học dùng lại được, cập nhật `memory/lessons-learned.md`.
- Nếu task làm thay đổi hiểu biết ổn định về người dùng/project, cập nhật `profile/`, `projects/`, hoặc `memory/` tương ứng.
