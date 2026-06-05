# Projects

Thư mục này lưu thông tin và tài liệu đầu vào theo từng project người dùng hay làm.

Đây là khu vực chứa tài liệu kỹ thuật, BRD, URD, hoặc tài liệu do BA/PM đưa để agent đọc trước khi làm task. Mỗi project có folder riêng để tránh trộn context giữa các dự án.

Mỗi project nên có:

- Mục tiêu/vai trò của project.
- Path source code nếu đã biết.
- `project-index.md` nếu project đã được index chi tiết.
- Tài liệu BRD/URD/technical docs liên quan.
- Stack hoặc cấu trúc riêng nếu khác profile chung.
- Task/lỗi/decision quan trọng liên quan đến project.
- Link tới file trong `tasks/`, `errors/`, hoặc `daily-report/` nếu có.

## Cấu trúc đề xuất cho mỗi project

```text
projects/<project-name>/
  README.md
  brd/
  urd/
  technical-docs/
```

Ý nghĩa:

- `brd/`: Business Requirement Document, thường từ BA/PM.
- `urd/`: User Requirement Document, yêu cầu từ góc nhìn người dùng/nghiệp vụ.
- `technical-docs/`: tài liệu kỹ thuật, integration, API, flow, architecture, constraints.

Khi làm task theo project, agent phải đọc đúng folder project đó trước. Không lấy tài liệu từ project khác trừ khi người dùng nói rõ có liên quan.

## Danh sách project hiện tại

| Project | Vai trò | Ghi chú |
| --- | --- | --- |
| `hdb_mfe_loan` | Tính năng vay | Project đang ưu tiên. |
| `hdb_host` | Host cho mini app | Path: `/Users/atien/work/app/hdb_host`. |
| `hdb_mfe_dop` | Phát hành thẻ | Path: `/Users/atien/work/app/hdb_mfe_dop`. |
| `hdb_mfe_od` | Vay thấu chi | Path: `/Users/atien/work/app/hdb_mfe_od`. Người dùng không làm chính, tài liệu sẽ bổ sung sau. |
