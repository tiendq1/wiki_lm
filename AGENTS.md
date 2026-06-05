# WikiLLM Agent Guide

File này mô tả cách agent cá nhân làm việc trong workspace này.

Mục tiêu chính:

- Hiểu người dùng tốt hơn qua thông tin được bổ sung hằng ngày.
- Đọc nhiệm vụ trong `tasks/`, thực hiện nhiệm vụ, rồi cập nhật kết quả vào `daily-report/`.
- Ghi lại lỗi, cách sửa lỗi, và command line quan trọng để có thể học lại từ lịch sử làm việc.

## Nguyên tắc chung

- Luôn đọc `AGENTS.md` trước khi làm việc trong workspace này.
- Ưu tiên thông tin do người dùng cung cấp trực tiếp.
- Nếu thông tin chưa đủ, tự đưa ra giả định an toàn và ghi rõ giả định trong báo cáo.
- Không tự ý xoá hoặc ghi đè thông tin cũ nếu chưa được yêu cầu.
- Khi có thay đổi quan trọng về quy trình, cập nhật lại `AGENTS.md`.

## Lệnh `wiki`

Khi người dùng nói hoặc gõ `wiki`, agent phải dùng `WIKI.md` làm entrypoint.

Thứ tự đọc mặc định:

1. `WIKI.md`
2. `memory/current-context.md`
3. `memory/stable-memory.md`
4. `profile/communication-style.md`
5. `profile/preferences.md`
6. `profile/tech-stack.md`
7. `profile/current-projects.md`
8. `memory/lessons-learned.md`

Sau khi đọc lớp bootstrap này, chỉ đọc sâu file/folder liên quan đến task hiện tại.

## Quyền thao tác và bảo mật

- Khi cần scan project, đọc file, dò lỗi, hoặc chạy command để mò fixbug: agent được phép làm, không cần hỏi lại trước.
- Nếu thao tác có khả năng liên quan đến bảo mật, secret, token, credential, thông tin nội bộ, dữ liệu nhạy cảm, hoặc có khả năng rò rỉ dữ liệu ra bên ngoài: phải hỏi người dùng confirm trước.
- Nếu cần dùng network, gửi log/code/data ra dịch vụ bên ngoài, upload file, cài package từ nguồn bên ngoài, hoặc gọi API ngoài workspace: phải hỏi người dùng confirm trước.
- Nếu không chắc thao tác có rủi ro rò rỉ thông tin hay không, dừng lại và hỏi ngắn gọn.

## Nơi lấy nhiệm vụ

Nhiệm vụ được đặt trong thư mục `tasks/`.

Chỉ lưu yêu cầu vào `tasks/` khi người dùng nói rõ đó là task hoặc yêu cầu "ghi task/tạo task/lưu task". Các yêu cầu nạp data, bổ sung context, cập nhật profile, cập nhật memory, hoặc chỉnh tài liệu WikiLLM không tự động trở thành task.

Khi cần tạo task mới, dùng template ở `templates/task-template.md`.

Cấu trúc hiện tại:

- `tasks/web/`: nhiệm vụ liên quan đến web.
- `tasks/mobile/`: nhiệm vụ liên quan đến mobile.
- Các file nhiệm vụ khác trong `tasks/` có thể được thêm dần.

Khi bắt đầu làm việc:

1. Kiểm tra các file trong `tasks/`.
2. Đọc nhiệm vụ liên quan.
3. Xác định loại nhiệm vụ: web, mobile, tài liệu, nghiên cứu, hay việc cá nhân.
4. Thực hiện nhiệm vụ theo phạm vi được mô tả.
5. Sau khi hoàn thành, cập nhật báo cáo ngày trong `daily-report/`.

## Nơi lưu thông tin và tài liệu project

Thông tin và tài liệu đầu vào của các project người dùng hay làm được đặt trong `projects/`.

Khu vực này có thể chứa tài liệu kỹ thuật, BRD, URD, hoặc tài liệu từ BA/PM đưa để agent làm task. Mỗi project có folder riêng để tránh trộn context.

Khi task có liên quan đến một project cụ thể:

1. Đọc `projects/<project-name>/README.md` nếu có.
2. Đọc tài liệu liên quan trong project đó:
   - `projects/<project-name>/brd/`
   - `projects/<project-name>/urd/`
   - `projects/<project-name>/technical-docs/`
3. Đọc thêm `profile/current-projects.md` để biết project nào đang ưu tiên.
4. Nếu phát hiện thông tin project mới quan trọng, cập nhật lại file project tương ứng khi người dùng yêu cầu hoặc khi việc cập nhật thuộc phạm vi WikiLLM.

Không dùng tài liệu của project khác để suy luận cho task hiện tại, trừ khi người dùng nói rõ có liên quan.

## Cập nhật daily report

Sau khi hoàn thành một nhiệm vụ, cập nhật file:

```text
daily-report/YYYY-MM-DD.md
```

Dùng ngày hiện tại theo timezone của người dùng.

Nếu file chưa tồn tại, tạo file mới.

Mẫu ghi nhận:

```md
## Task: <tên nhiệm vụ>

- Nguồn: `tasks/...`
- Loại: web | mobile | personal | docs | research | other
- Trạng thái: completed | partial | blocked
- Đã làm:
  - ...
- Kết quả:
  - ...
- File/thư mục đã thay đổi:
  - ...
- Ghi chú cho trí nhớ agent:
  - ...
```

## Ghi lỗi

Lỗi được ghi trong thư mục `errors/`.

- Lỗi web: ghi vào `errors/web/`.
- Lỗi mobile: ghi vào `errors/mobile/`.

Nếu gặp hoặc sửa lỗi mobile, tạo hoặc cập nhật file:

```text
errors/mobile/YYYY-MM-DD.md
```

Mẫu ghi lỗi:

```md
## <tên lỗi>

- Thời điểm:
- Nguồn/task:
- Triệu chứng:
- Nguyên nhân:
- Cách sửa:
- Trạng thái:
- Bài học:
```

## Ghi command line khi sửa lỗi

Trong quá trình sửa lỗi, nếu có dùng command line quan trọng, ghi vào:

```text
terminal/fixbug.md
```

Mẫu ghi command:

````md
## YYYY-MM-DD - <task/lỗi>

### Command

```sh
<command đã chạy>
```

### Mục đích

- ...

### Kết quả

- ...
````

Chỉ cần ghi các command có ý nghĩa cho việc debug/sửa lỗi. Không cần ghi mọi command đọc file thông thường nếu không liên quan đến sửa lỗi.

## Thông tin bổ sung dần

Người dùng sẽ cung cấp thêm thông tin theo thời gian. Khi có thông tin mới:

- Nếu là thông tin về thói quen, sở thích, cách giao tiếp, mục tiêu cá nhân: cân nhắc lưu vào vùng memory/profile khi cấu trúc đó được tạo.
- Nếu là thông tin theo ngày: lưu vào `daily-report/`.
- Nếu là thông tin liên quan đến một nhiệm vụ đã được người dùng gọi rõ là task: cập nhật task hoặc daily report tương ứng.
- Nếu là bài học từ lỗi: cập nhật `errors/` và `terminal/fixbug.md` nếu có command line.
- Nếu là thông tin ổn định/tái sử dụng nhiều lần: cập nhật thêm `memory/`.
- Nếu người dùng chỉ nạp data/context/tài liệu để agent hiểu về sau, không coi đó là task và không ghi vào `daily-report/` trừ khi người dùng yêu cầu report rõ ràng.

## Cách phản hồi với người dùng

- Báo ngắn gọn việc đã làm.
- Nếu có file được cập nhật, nêu rõ đường dẫn.
- Nếu bị chặn, nói rõ lý do và cần người dùng cung cấp gì.
- Ưu tiên hành động thực tế hơn là giải thích dài.
