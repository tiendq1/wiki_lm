# Current Projects

## Ưu tiên hiện tại

Ưu tiên chính hiện tại là React Native.

Công việc chính hiện tại: tính năng vay cầm cố bằng sổ tiết kiệm.

Thông tin chi tiết từng project được lưu thêm trong `projects/`.

## `hdb_host`

- Path: `/Users/atien/work/app/hdb_host`
- Vai trò:
  - Host cho mini app chạy được.
- Các tính năng vay trong host:
  - Đăng ký nhu cầu vay.
  - Vay nhanh.
  - Vay thấu chi.
  - Vay cầm cố sổ tiết kiệm.
- Ghi chú:
  - Đây là project đã có lỗi iOS simulator crash được ghi lại trong `errors/mobile/2026-06-03-hdb-host-simulator-crash.md`.

## `hdb_mfe_loan`

- Path: `/Users/atien/work/app/hdb_mfe_loan`
- Trạng thái ưu tiên: dự án đang ưu tiên.
- Vai trò:
  - Làm tính năng vay cho app bank.
- Tính năng chính:
  - Vay cầm cố bằng sổ tiết kiệm.
- Ghi chú:
  - Bản cũ làm bằng web React JS và nhúng vào WebView của native mobile.
  - Bản mới làm bằng React Native TypeScript, là miniapp cho tính năng vay, được view/host vào `hdb_host`.

## `hdb_mfe_dop`

- Path: `/Users/atien/work/app/hdb_mfe_dop`
- Vai trò:
  - Phát hành thẻ.

## `hdb_mfe_od`

- Path: chưa bổ sung.
- Vai trò:
  - Vay thấu chi.

## Cách agent nên xử lý project ưu tiên

- Khi người dùng nói về task mobile mà không nói rõ project, cần cân nhắc `hdb_mfe_loan` là project ưu tiên trước.
- Nếu task liên quan host/mini app/runtime/container, cân nhắc `hdb_host`.
- Nếu cần scan project để hiểu bug hoặc context, có thể đọc/dò/run command điều tra theo quyền người dùng đã nêu.
- Nếu thao tác có rủi ro security, secret, dữ liệu nhạy cảm, network, upload, external API, hoặc rò rỉ thông tin ra ngoài, hỏi người dùng confirm trước.
