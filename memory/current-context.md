# Current Context

## Ưu tiên hiện tại

- Trọng tâm công việc hiện tại: React Native.
- Project ưu tiên: `hdb_mfe_loan`.
- Công việc chính hiện tại: tính năng vay cầm cố bằng sổ tiết kiệm.
- Bản cũ: web React JS, nhúng vào WebView của native mobile.
- Source bản web vay cầm cố STK: `/Users/atien/work/web/lending_uat_2026`.
- Endpoint group bản web vay cầm cố STK: `/api-dl/*`.
- Bản mới: React Native TypeScript miniapp, được view/host vào `hdb_host`.

## Project đang theo dõi

| Project | Vai trò | Path | Trạng thái |
| --- | --- | --- | --- |
| `hdb_mfe_loan` | Tính năng vay | `/Users/atien/work/app/hdb_mfe_loan` | Đang ưu tiên |
| `hdb_host` | Host cho mini app | `/Users/atien/work/app/hdb_host` | Có lỗi simulator crash đã ghi lại |
| `hdb_mfe_dop` | Phát hành thẻ | `/Users/atien/work/app/hdb_mfe_dop` | Đã có path |
| `hdb_mfe_od` | Vay thấu chi | `/Users/atien/work/app/hdb_mfe_od` | Không làm chính, thiếu tài liệu |

## Cách dùng `projects/`

- `projects/` là nơi lưu tài liệu kỹ thuật, BRD, URD từ BA/PM theo từng project.
- Khi làm task theo project, đọc đúng folder project trước.
- Không trộn tài liệu giữa `hdb_mfe_loan`, `hdb_mfe_dop`, `hdb_mfe_od` nếu người dùng chưa nói có liên quan.

## BRD đã nạp

- `projects/hdb_mfe_loan/brd/vay-cam-co-stk-online-registration-web-brd.md`
- `projects/hdb_mfe_loan/brd/mb02-brd-nang-hm-vay-cc-stk-online.md`
- Nguồn: `documents/BRD NANG HM VAY CC STK ONLINE (HDB VNP).pdf`
- Nguồn mới: `documents/MB02-BRD NANG HM VAY CC STK ONLINE (HDB VNP).docx`
- Phạm vi nạp: đăng ký vay cầm cố sổ tiết kiệm.
- Không nạp hiện tại: tất toán khoản vay.
- Lưu ý mapping OTP:
  - Bản web ReactJS/BRD có màn hình nhập OTP riêng.
  - Bản mobile React Native hiện tại chỉ có popup/bottom sheet OTP.
  - Khi làm task React Native, ưu tiên wording và flow của mobile design/source.

## Project index đã tạo

- `projects/hdb_mfe_loan/project-index.md`
- Nội dung: source path, package scripts, command chạy/build/test, module vay cầm cố STK, flow màn hình chính, source files, đối chiếu BRD.

## Technical docs đã nạp

- `projects/hdb_mfe_loan/technical-docs/mb03-digital-lending-technical-spec.md`
  - Nguồn: `documents/MB03 - Phieu_Dac_Ta_Ky_Thuat_Digital_Lending_EDIT.docx`
  - Nội dung: kiến trúc WebApp/GWDL/ESB/FPT, flow kỹ thuật, API fields, JWT/AES flow.
- `projects/hdb_mfe_loan/technical-docs/postman-webapp-digital-lending-api-index.md`
  - Nguồn: `documents/Webapp Digital Lending - Pentest.postman_collection.json`
  - Nội dung: index API Postman đã redact secret/token/API key.
- `projects/hdb_mfe_loan/technical-docs/error-message-map.md`
  - Nguồn: `documents/errorMessage.json`
  - Nội dung: error code/message cho Digital Lending.
- Lưu ý bảo mật: Postman collection có secret/token/API key, không copy raw value vào wiki/chat/log.

## Open questions

- `memory/open-question.md`
- Nội dung: các điểm còn thiếu về API spec, field mapping, design mobile, host integration, và tài liệu chưa có.

## Việc cần bổ sung

- Tài liệu BRD/URD/technical docs cho từng project khi người dùng có.
- Nếu có task mới, đặt vào `tasks/` theo nhóm web/mobile hoặc theo project nếu sau này mở rộng cấu trúc.
