# Fix Bug Command Log

File này lưu các command line quan trọng được dùng trong quá trình debug hoặc sửa lỗi.

## 2026-06-04 - hdb_host simulator launch crash

### Phạm vi

- Project: `hdb_host`
- Path: `/Users/atien/work/app/hdb_host`
- Lỗi: app crash/fail khi mở trên iOS simulator.
- Ghi chú: đây là các command đã dùng để scan lại, xác định nguyên nhân, và xác nhận hướng fix. Không ghi thêm command fix cũ nếu không có log xác nhận.

### Command

Các command dưới đây chạy trong `/Users/atien/work/app/hdb_host`, trừ command có path tuyệt đối.

```sh
ls -la /Users/atien/work/app/hdb_host

git status --short
git diff --stat
git diff -- react-native.config.js package.json yarn.lock ios android src index.ts babel.config.js
git log --oneline --decorate -8

sed -n '1,80p' react-native.config.js
sed -n '1,220p' ios/Podfile
sed -n '150,190p' ios/hdb_host/Info.plist

rg -n "esign|easyca|SDKCA|bshield|AuthenticationIDSDK|TrueIDFaceDetectSDK|UIRequiredDeviceCapabilities|iphonesimulator" -S .

find build ios -maxdepth 4 -type f -iname '*log*' -print
rg -n "dyld|Library not loaded|SDKCA|AuthenticationIDSDK|TrueIDFaceDetectSDK|bshield|iPhoneOS|iphonesimulator|simulator|crash|failed|unsupported|arm64" build ios -S
find ios -maxdepth 4 -type f -name 'Pods-hdb_host-frameworks*' -print

rg -n "bshield|AuthenticationIDSDK|TrueIDFaceDetectSDK|SDKCA|install_framework" ios/Pods/Target\ Support\ Files/Pods-hdb_host/Pods-hdb_host-frameworks.sh ios/Pods/Target\ Support\ Files/Pods-hdb_host/Pods-hdb_host-frameworks-Debug-input-files.xcfilelist ios/Pods/Target\ Support\ Files/Pods-hdb_host/Pods-hdb_host-frameworks-Debug-output-files.xcfilelist

git diff -- ios/Podfile react-native.config.js ios/hdb_host/Info.plist package.json
find node_modules/@dev-dtc/react-native-esign-easyca-sdk -maxdepth 3 -type f -print

file node_modules/@dev-dtc/react-native-esign-easyca-sdk/ios/SDKCA.framework/SDKCA ios/Pods/HDBLibs/bshield.framework/bshield ios/Pods/HDBLibs/VNeIDSDK/AuthenticationIDSDK.framework/AuthenticationIDSDK ios/Pods/HDBLibs/VNeIDSDK/TrueIDFaceDetectSDK.framework/TrueIDFaceDetectSDK

sed -n '1,120p' node_modules/@dev-dtc/react-native-esign-easyca-sdk/EsignEasyca.podspec
sed -n '172,194p' ios/Pods/Target\ Support\ Files/Pods-hdb_host/Pods-hdb_host-frameworks.sh
```

### Công dụng từng command

#### `ls -la /Users/atien/work/app/hdb_host`

- Kiểm tra project `hdb_host` có tồn tại đúng path không.
- Xem nhanh cấu trúc repo, các folder/file chính, và thời điểm file thay đổi gần nhất.

#### `git status --short`

- Xem project đang có file nào bị thay đổi.
- Xác định nhanh những file có khả năng liên quan tới lần fix crash.

#### `git diff --stat`

- Xem tổng quan diff: file nào thay đổi, số dòng thêm/xóa.
- Dùng để nhận diện phạm vi fix trước khi đọc diff chi tiết.

#### `git diff -- react-native.config.js package.json yarn.lock ios android src index.ts babel.config.js`

- Xem diff chi tiết ở các file/folder nghi liên quan React Native/iOS/dependency.
- Mục tiêu là tìm chính xác file nào đã sửa để fix simulator crash.

#### `git log --oneline --decorate -8`

- Xem các commit gần nhất để hiểu ngữ cảnh branch/repo.
- Kiểm tra có commit nào liên quan crash/deeplink/build/native không.

#### `sed -n '1,80p' react-native.config.js`

- Đọc phần đầu `react-native.config.js`.
- Xác nhận có cấu hình tắt autolink iOS cho `@dev-dtc/react-native-esign-easyca-sdk`.

#### `sed -n '1,220p' ios/Podfile`

- Đọc `ios/Podfile`.
- Kiểm tra logic CocoaPods patch embed frameworks cho simulator.

#### `sed -n '150,190p' ios/hdb_host/Info.plist`

- Đọc vùng `Info.plist` quanh `UILaunchStoryboardName` và supported orientations.
- Xác nhận `UIRequiredDeviceCapabilities -> arm64` đã bị xóa.

#### `rg -n "esign|easyca|SDKCA|bshield|AuthenticationIDSDK|TrueIDFaceDetectSDK|UIRequiredDeviceCapabilities|iphonesimulator" -S .`

- Search toàn project các keyword liên quan EasyCA, SDKCA, HDBLibs, framework device-only, simulator, và `Info.plist`.
- Dùng để tìm mọi chỗ có khả năng gây crash/fail khi chạy simulator.

#### `find build ios -maxdepth 4 -type f -iname '*log*' -print`

- Tìm log file trong `build/` và `ios/`.
- Mục tiêu là xem có build/launch log nào còn lưu dấu vết crash hoặc lỗi framework.

#### `rg -n "dyld|Library not loaded|SDKCA|AuthenticationIDSDK|TrueIDFaceDetectSDK|bshield|iPhoneOS|iphonesimulator|simulator|crash|failed|unsupported|arm64" build ios -S`

- Search log/build artifacts theo các keyword crash native thường gặp.
- Tìm dấu hiệu như `dyld`, `Library not loaded`, unsupported architecture, hoặc framework device-only.
- Ghi chú: command này output rất lớn vì search trong `build/`; lần sau nên scope hẹp hơn nếu có thể.

#### `find ios -maxdepth 4 -type f -name 'Pods-hdb_host-frameworks*' -print`

- Tìm các generated CocoaPods files liên quan embed frameworks.
- Mục tiêu là xác định file script/filelist nào cần kiểm tra.

#### `rg -n "bshield|AuthenticationIDSDK|TrueIDFaceDetectSDK|SDKCA|install_framework" ios/Pods/Target\ Support\ Files/Pods-hdb_host/Pods-hdb_host-frameworks.sh ios/Pods/Target\ Support\ Files/Pods-hdb_host/Pods-hdb_host-frameworks-Debug-input-files.xcfilelist ios/Pods/Target\ Support\ Files/Pods-hdb_host/Pods-hdb_host-frameworks-Debug-output-files.xcfilelist`

- Search trực tiếp trong CocoaPods embed framework script và filelist.
- Kiểm tra các framework device-only còn nằm trong embed phase hay đã bị guard/lọc khỏi simulator.

#### `git diff -- ios/Podfile react-native.config.js ios/hdb_host/Info.plist package.json`

- Xem diff gọn ở các file chính đã sửa.
- Dùng để tách rõ: autolink fix, Podfile fix, Info.plist fix, dependency version fix.

#### `find node_modules/@dev-dtc/react-native-esign-easyca-sdk -maxdepth 3 -type f -print`

- Xem cấu trúc package EasyCA trong `node_modules`.
- Tìm `EsignEasyca.podspec` và `ios/SDKCA.framework`.

#### `file node_modules/@dev-dtc/react-native-esign-easyca-sdk/ios/SDKCA.framework/SDKCA ios/Pods/HDBLibs/bshield.framework/bshield ios/Pods/HDBLibs/VNeIDSDK/AuthenticationIDSDK.framework/AuthenticationIDSDK ios/Pods/HDBLibs/VNeIDSDK/TrueIDFaceDetectSDK.framework/TrueIDFaceDetectSDK`

- Kiểm tra loại binary của các framework nghi gây lỗi.
- Kết quả cho thấy các binary là Mach-O arm64, tức framework device-oriented, không phù hợp để embed vào iOS simulator build.

#### `sed -n '1,120p' node_modules/@dev-dtc/react-native-esign-easyca-sdk/EsignEasyca.podspec`

- Đọc podspec của EasyCA.
- Xác nhận package khai báo `vendored_frameworks = 'ios/SDKCA.framework'`, tức CocoaPods có thể kéo framework này vào iOS native build.

#### `sed -n '172,194p' ios/Pods/Target\ Support\ Files/Pods-hdb_host/Pods-hdb_host-frameworks.sh`

- Đọc đoạn generated script embed frameworks.
- Xác nhận `bshield.framework`, `AuthenticationIDSDK.framework`, `TrueIDFaceDetectSDK.framework` đã có guard: chỉ install khi không phải `iphonesimulator`, còn simulator thì remove khỏi app bundle.

### Mục đích tổng quát

- Kiểm tra project `hdb_host` có tồn tại và xem trạng thái thay đổi hiện tại.
- Xem diff để biết các file từng được sửa liên quan crash simulator.
- Kiểm tra `react-native.config.js`, `ios/Podfile`, `Info.plist`, `package.json`, `yarn.lock`, `Podfile.lock`.
- Tìm dấu vết EasyCA/SDKCA/HDBLibs/device-only frameworks.
- Xác nhận binary `SDKCA`, `bshield`, `AuthenticationIDSDK`, `TrueIDFaceDetectSDK` là Mach-O arm64 framework.
- Xác nhận `Pods-hdb_host-frameworks.sh` đã có guard không embed framework device-only khi build `iphonesimulator`.

### Kết quả

- Nguyên nhân chính: native framework chỉ dành cho device thật bị autolink/embed vào iOS simulator build.
- Fix đã ghi trong `errors/mobile/2026-06-03-hdb-host-simulator-crash.md`:
  - Tắt iOS autolink cho `@dev-dtc/react-native-esign-easyca-sdk`.
  - Patch `ios/Podfile` để loại framework device-only khỏi simulator embed phase.
  - Xóa `UIRequiredDeviceCapabilities -> arm64` khỏi `Info.plist`.
  - Cập nhật dependency/lockfile liên quan.

### Bài học

- Nếu là bug fix, ghi command line quan trọng vào `terminal/fixbug.md`.
- Không cần ghi mọi command đọc file bình thường, nhưng cần ghi các command giúp tìm nguyên nhân, xác minh giả thuyết, hoặc xác nhận fix.
- Tránh search quá rộng trong `build/` nếu không cần; output rất lớn và khó đọc. Nên search scoped theo file/folder liên quan trước.

## Mẫu ghi nhận

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
