# Lessons Learned

## Mobile / React Native

### iOS simulator crash trước khi JS load

- Project liên quan: `hdb_host`.
- Lỗi đã ghi: `errors/mobile/2026-06-03-hdb-host-simulator-crash.md`.
- Command debug đã ghi: `terminal/fixbug.md`.

Kinh nghiệm:

- Nếu app React Native crash/fail rất sớm trên iOS simulator, kiểm tra native vendored frameworks trước.
- Các SDK ngân hàng/eKYC/eSign có thể chứa binary chỉ build cho device thật.
- Framework device-only không nên autolink/embed vào simulator build.
- Với CocoaPods generated scripts, cần kiểm tra cả:
  - `Pods-*-frameworks.sh`
  - `Pods-*-frameworks-*.xcfilelist`
- Nếu chỉ patch script mà filelist vẫn giữ framework, Xcode có thể tiếp tục xử lý framework đó.

Cách xử lý đã dùng ở `hdb_host`:

- Tắt iOS autolink cho `@dev-dtc/react-native-esign-easyca-sdk`.
- Patch `ios/Podfile` để framework device-only không embed khi `PLATFORM_NAME=iphonesimulator`.
- Xóa `UIRequiredDeviceCapabilities -> arm64` khỏi `Info.plist`.
- Cập nhật dependency/lockfile liên quan.

## Knowledge Management

- Khi ghi lỗi, không chỉ liệt kê file đã sửa.
- Cần ghi rõ từng file:
  - Sửa gì
  - Vì sao sửa
  - Vai trò trong lỗi
  - Cách kiểm tra
- Với fix bug, ghi command line và công dụng từng command vào `terminal/fixbug.md`.

## Web / Security

### Secret key encrypt body không được nằm trên FE

- Task liên quan: `tasks/web/2026-06-05-pentest-secret-key-encrypt-body-fe.md`.
- Lỗi đã ghi: `errors/security/2026-06-05-pentest-secret-key-encrypt-body-fe.md`.

Kinh nghiệm:

- Frontend là môi trường public/untrusted. Secret xuất hiện trong source, bundle, sourcemap, DevTools, hoặc runtime đều phải coi như đã lộ.
- Encrypt body ở FE không có giá trị bảo mật mạnh nếu key encrypt cũng nằm trong FE.
- Nếu cần bảo vệ payload:
  - Đưa logic dùng secret về backend/service tin cậy.
  - Dùng public-key flow nếu FE bắt buộc phải encrypt.
  - Tránh key tĩnh dùng chung nhiều user/session.
  - Rotate key đã từng bị expose.
- Khi xử lý pentest dạng này, kiểm tra cả source, env build-time, production bundle, sourcemap, log, và request flow.
