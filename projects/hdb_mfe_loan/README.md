# hdb_mfe_loan

## Vai trò

- Làm tính năng vay cho app bank.
- Tính năng chính người dùng đang làm: vay cầm cố bằng sổ tiết kiệm.

## Trạng thái

- Project đang ưu tiên.

## Path

```text
/Users/atien/work/app/hdb_mfe_loan
```

## Ghi chú cho agent

- Khi người dùng nói task mobile mà không nói rõ project, cân nhắc `hdb_mfe_loan` trước.
- Bản cũ của tính năng vay cầm cố STK từng làm bằng web React JS và nhúng vào WebView của native mobile.
- Source webapp vay cầm cố STK: `/Users/atien/work/web/lending_uat_2026`.
- Bản mới làm bằng React Native TypeScript, là miniapp cho tính năng vay, được view/host vào `hdb_host`.
- Nếu cần scan/dò/run command để hiểu bug local, được phép làm theo rule trong `AGENTS.md`.
- Nếu thao tác có rủi ro bảo mật, secret, dữ liệu nhạy cảm, network, upload, external API, hoặc rò rỉ thông tin ra ngoài, hỏi người dùng confirm trước.

## Tài liệu đầu vào

- `brd/`: BRD từ BA/PM.
- `urd/`: URD hoặc yêu cầu người dùng/nghiệp vụ.
- `technical-docs/`: tài liệu kỹ thuật, API, flow, architecture, constraints.

Khi làm task của project này, đọc tài liệu trong các folder trên trước nếu có.

## Project index

- `project-index.md`: source path, commands, scripts, module chính, flow màn hình, source files, và đối chiếu BRD.

## BRD đã nạp

- `brd/vay-cam-co-stk-online-registration-web-brd.md`: digest BRD bản web cho phần đăng ký vay cầm cố sổ tiết kiệm online. Không bao gồm phần tất toán.
- `brd/mb02-brd-nang-hm-vay-cc-stk-online.md`: digest BRD MB02 từ file docx mới.
- `technical-docs/brd-api-flow-business-rules.md`: tóm tắt từ BRD về API/integration được nhắc đến, flow vay hiện tại, và rule nghiệp vụ chính.
- `technical-docs/mb03-digital-lending-technical-spec.md`: digest tài liệu kỹ thuật MB03.
- `technical-docs/postman-webapp-digital-lending-api-index.md`: index API từ Postman collection, đã redact secret/token/API key.
