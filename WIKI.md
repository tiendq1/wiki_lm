# WikiLLM Bootstrap

File này là entrypoint khi người dùng nói hoặc gõ `wiki`.

Mục tiêu: giúp agent nắm nhanh người dùng là ai, đang ưu tiên project nào, đã có kinh nghiệm/lỗi nào, và cần đọc tài liệu ở đâu trước khi làm việc.

## Khi người dùng gõ `wiki`

Agent đọc theo thứ tự:

1. `WIKI.md`
2. `memory/current-context.md`
3. `memory/stable-memory.md`
4. `profile/communication-style.md`
5. `profile/preferences.md`
6. `profile/tech-stack.md`
7. `profile/current-projects.md`
8. `memory/lessons-learned.md`

Sau đó chỉ đọc sâu phần liên quan đến task hiện tại.

## Cách đọc theo loại việc

### Nếu là task theo project

Chỉ coi là task nếu người dùng nói rõ đó là task hoặc yêu cầu lưu/tạo task.

1. Xác định project.
2. Đọc `projects/<project-name>/README.md`.
3. Đọc tài liệu trong project đó nếu có:
   - `projects/<project-name>/brd/`
   - `projects/<project-name>/urd/`
   - `projects/<project-name>/technical-docs/`
4. Đọc task liên quan trong `tasks/`.
5. Nếu có bug/lỗi liên quan, đọc `errors/` và `terminal/fixbug.md`.

Không dùng tài liệu của project khác trừ khi người dùng nói rõ có liên quan.

### Nếu là bug fix

1. Đọc task trong `tasks/`.
2. Đọc project context trong `projects/`.
3. Scan source project theo rule bảo mật trong `AGENTS.md`.
4. Ghi lỗi vào `errors/mobile/` hoặc `errors/web/`.
5. Ghi command line quan trọng vào `terminal/fixbug.md`.
6. Cập nhật `daily-report/YYYY-MM-DD.md`.

### Nếu là tư vấn/kế hoạch/cách làm

1. Đọc profile và memory trước.
2. Trả lời ngắn, kỹ thuật, nói thẳng.
3. Nếu thiếu dữ kiện quan trọng, hỏi đúng phần thiếu.

## Lớp dữ liệu

| Khu vực | Vai trò |
| --- | --- |
| `profile/` | Thông tin về người dùng: nghề nghiệp, stack, style giao tiếp, mục tiêu. |
| `memory/` | Tóm tắt ổn định và context hiện tại để đọc nhanh. |
| `projects/` | Tài liệu theo project: BRD, URD, technical docs, context BA/PM. |
| `tasks/` | Việc cần làm. |
| `errors/` | Lỗi đã gặp, nguyên nhân, cách sửa, bài học. |
| `terminal/fixbug.md` | Command line quan trọng khi debug/fix bug. |
| `daily-report/` | Nhật ký cập nhật hằng ngày. |
| `monthly-reports/` | Tổng kết theo tháng khi cần. |

## Cách cập nhật sau khi làm việc

- Thông tin ổn định về người dùng: cập nhật `profile/` hoặc `memory/stable-memory.md`.
- Ưu tiên hiện tại/project hiện tại: cập nhật `memory/current-context.md`.
- Lỗi/kỹ thuật đã học: cập nhật `errors/`, `terminal/fixbug.md`, và `memory/lessons-learned.md` nếu bài học có thể tái sử dụng.
- Việc đã làm trong ngày dưới dạng task thật: cập nhật `daily-report/YYYY-MM-DD.md`.
- Tài liệu BA/PM/technical docs: đặt đúng folder trong `projects/<project-name>/`.
- Nạp data/context/tài liệu để agent hiểu về sau không tự động ghi daily report, trừ khi người dùng yêu cầu.

## Rule về task

- Chỉ lưu vào `tasks/` khi người dùng nói rõ đó là task.
- Nếu người dùng chỉ nói "nạp data", "bổ sung context", "ghi nhớ", "update profile", hoặc "lưu tài liệu", không tạo task.
- Khi cần tạo task mới, dùng template ở `templates/task-template.md`.

## Rule quan trọng

- Debug local: được scan, đọc file, chạy command để mò bug.
- Security/data leakage/network/upload/external API: hỏi người dùng confirm trước.
- Không bịa log, command, nguyên nhân, hay lịch sử sửa lỗi.
- Không trộn context giữa project nếu người dùng chưa nói có liên quan.
