# Current Context

## Ưu tiên hiện tại

- Trọng tâm công việc hiện tại: React Native.
- Project ưu tiên: `hdb_mfe_loan`.
- Công việc chính hiện tại: tính năng vay cầm cố bằng sổ tiết kiệm.
- Bản cũ: web React JS, nhúng vào WebView của native mobile.
- Bản mới: React Native TypeScript miniapp, được view/host vào `hdb_host`.

## Project đang theo dõi

| Project | Vai trò | Path | Trạng thái |
| --- | --- | --- | --- |
| `hdb_mfe_loan` | Tính năng vay | `/Users/atien/work/app/hdb_mfe_loan` | Đang ưu tiên |
| `hdb_host` | Host cho mini app | `/Users/atien/work/app/hdb_host` | Có lỗi simulator crash đã ghi lại |
| `hdb_mfe_dop` | Phát hành thẻ | `/Users/atien/work/app/hdb_mfe_dop` | Đã có path |
| `hdb_mfe_od` | Vay thấu chi | Chưa bổ sung | Thiếu path |

## Cách dùng `projects/`

- `projects/` là nơi lưu tài liệu kỹ thuật, BRD, URD từ BA/PM theo từng project.
- Khi làm task theo project, đọc đúng folder project trước.
- Không trộn tài liệu giữa `hdb_mfe_loan`, `hdb_mfe_dop`, `hdb_mfe_od` nếu người dùng chưa nói có liên quan.

## BRD đã nạp

- `projects/hdb_mfe_loan/brd/vay-cam-co-stk-online-registration-web-brd.md`
- Nguồn: `documents/BRD NANG HM VAY CC STK ONLINE (HDB VNP).pdf`
- Phạm vi nạp: đăng ký vay cầm cố sổ tiết kiệm.
- Không nạp hiện tại: tất toán khoản vay.

## Việc cần bổ sung

- Path source cho `hdb_mfe_od`.
- Tài liệu BRD/URD/technical docs cho từng project khi người dùng có.
- Nếu có task mới, đặt vào `tasks/` theo nhóm web/mobile hoặc theo project nếu sau này mở rộng cấu trúc.
