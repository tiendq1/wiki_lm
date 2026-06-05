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
