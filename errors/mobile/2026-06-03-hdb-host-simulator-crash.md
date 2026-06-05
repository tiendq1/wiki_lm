# Mobile Error - hdb_host simulator launch crash

## Crash app khi mở trên simulator

- Thời điểm: ghi nhận vào WikiLLM ngày 2026-06-03
- Nguồn/task: `tasks/mobile/2026-06-03-hdb-host-simulator-crash.md`
- Dự án: `hdb_host`
- Đường dẫn dự án: `/Users/atien/work/app/hdb_host`
- Triệu chứng:
  - App crash khi mở trên simulator.
  - App fail rất sớm, trước khi JS load, khi iOS simulator bị embed/link các native framework chỉ build cho iPhoneOS.
- Nguyên nhân:
  - `@dev-dtc/react-native-esign-easyca-sdk` bundle `SDKCA.framework`; binary này là Mach-O arm64 device framework và không phù hợp để autolink vào iOS simulator build.
  - `HDBLibs` cũng có các framework device-only bị đưa vào embed frameworks của target app:
    - `bshield.framework`
    - `AuthenticationIDSDK.framework`
    - `TrueIDFaceDetectSDK.framework`
  - `Info.plist` trước đó có `UIRequiredDeviceCapabilities` với giá trị `arm64`, làm simulator/install path dễ bị ràng buộc sai cho môi trường dev.
- Cách sửa:
  - Cập nhật `react-native.config.js` để tắt autolinking iOS cho `@dev-dtc/react-native-esign-easyca-sdk`:
    - `dependencies['@dev-dtc/react-native-esign-easyca-sdk'].platforms.ios = null`
    - Lý do: `SDKCA.framework` trong package này chỉ dùng cho iPhoneOS, autolink vào simulator làm app fail trước khi JS load.
  - Cập nhật `ios/Podfile` để sau `pod install`, script `Pods-hdb_host-frameworks.sh` chỉ install các framework device-only khi `PLATFORM_NAME` không phải `iphonesimulator`.
  - Cập nhật `ios/Podfile` để lọc các framework device-only khỏi các file list `Pods-hdb_host-frameworks-*.xcfilelist`, tránh Xcode vẫn cố embed chúng khi build simulator.
  - Xóa `UIRequiredDeviceCapabilities -> arm64` khỏi `ios/hdb_host/Info.plist`.
  - Nâng `@dev-dtc/react-native-esign-easyca-sdk` trong `package.json` từ `0.0.6-uat.3` lên `0.0.9`, sau đó `yarn.lock` và `ios/Podfile.lock` được cập nhật theo dependency/pod mới.

## File đã sửa trong `hdb_host`

### Nhìn nhanh theo vai trò

| Nhóm | File | Đã sửa gì | Mục đích |
| --- | --- | --- | --- |
| React Native autolink | `/Users/atien/work/app/hdb_host/react-native.config.js` | Thêm cấu hình `dependencies` cho `@dev-dtc/react-native-esign-easyca-sdk` và set `platforms.ios = null`. | Không cho React Native autolink SDK này vào iOS target, vì `SDKCA.framework` chỉ phù hợp cho iPhoneOS/device thật. |
| CocoaPods embed frameworks | `/Users/atien/work/app/hdb_host/ios/Podfile` | Thêm danh sách `device_only_frameworks`; patch `Pods-hdb_host-frameworks.sh` để chỉ `install_framework` khi không phải `iphonesimulator`; lọc các framework đó khỏi `Pods-hdb_host-frameworks-*.xcfilelist`. | Giữ framework cho device thật nhưng loại khỏi simulator build để app không crash/fail trước khi JS load. |
| iOS app manifest | `/Users/atien/work/app/hdb_host/ios/hdb_host/Info.plist` | Xóa `UIRequiredDeviceCapabilities` với giá trị `arm64`. | Tránh ràng buộc capability/architecture không cần thiết khi chạy app trong simulator. |
| JS dependency | `/Users/atien/work/app/hdb_host/package.json` | Nâng `@dev-dtc/react-native-esign-easyca-sdk` từ `0.0.6-uat.3` lên `0.0.9`. | Dùng bản SDK mới hơn trong quá trình xử lý lỗi simulator. |
| JS dependency lock | `/Users/atien/work/app/hdb_host/yarn.lock` | Cập nhật entry lock của `@dev-dtc/react-native-esign-easyca-sdk` sang `0.0.9`, gồm `resolved` và `integrity`. | Khóa đúng version package sau khi đổi `package.json`. |
| CocoaPods lock | `/Users/atien/work/app/hdb_host/ios/Podfile.lock` | Loại `EsignEasyca` khỏi dependency/pod lock; cập nhật `hdb-core-service` từ `1.0.156-uat.1` lên `1.0.156-uat.2`; cập nhật checksum liên quan. | Phản ánh kết quả sau khi tắt autolink iOS cho EasyCA và chạy lại pod/dependency resolution. |

### Chi tiết từng file

#### `react-native.config.js`

File này là điểm chặn autolink ở tầng React Native.

Đã thêm:

```js
dependencies: {
  '@dev-dtc/react-native-esign-easyca-sdk': {
    platforms: {
      ios: null,
    },
  },
},
```

Ý nghĩa:

- React Native sẽ không tự đưa `@dev-dtc/react-native-esign-easyca-sdk` vào iOS native project.
- Tránh việc CocoaPods kéo `EsignEasyca` và `SDKCA.framework` vào simulator build.
- Đây là fix chính cho nhánh lỗi liên quan `SDKCA.framework`.

#### `ios/Podfile`

File này xử lý phần CocoaPods vẫn có thể embed framework device-only vào app bundle.

Đã thêm logic cho các framework chỉ dành cho device thật:

```text
bshield.framework
AuthenticationIDSDK.framework
TrueIDFaceDetectSDK.framework
```

Đã sửa theo 2 lớp:

- Patch `Pods-hdb_host-frameworks.sh`: nếu `PLATFORM_NAME` là `iphonesimulator` thì không install các framework trên, đồng thời remove framework khỏi app bundle nếu đã tồn tại.
- Patch `Pods-hdb_host-frameworks-*.xcfilelist`: lọc các dòng chứa framework device-only để Xcode không tiếp tục coi chúng là input/output của embed phase.

Ý nghĩa:

- Device thật vẫn dùng được các framework của `HDBLibs`.
- Simulator không bị nhúng binary device-only.
- Giảm khả năng app fail rất sớm trước khi JS bundle chạy.

#### `ios/hdb_host/Info.plist`

Đã xóa:

```xml
<key>UIRequiredDeviceCapabilities</key>
<array>
  <string>arm64</string>
</array>
```

Ý nghĩa:

- App không còn tự khai báo bắt buộc capability `arm64`.
- Tránh thêm một lớp ràng buộc không cần thiết khi install/chạy trên simulator.

#### `package.json`

Đã đổi:

```text
@dev-dtc/react-native-esign-easyca-sdk: 0.0.6-uat.3 -> 0.0.9
```

Ý nghĩa:

- Dùng bản EasyCA SDK mới hơn.
- Kết hợp với `react-native.config.js` để không autolink iOS package này vào simulator build.

#### `yarn.lock`

Đã cập nhật lock entry cho:

```text
@dev-dtc/react-native-esign-easyca-sdk@0.0.9
```

Ý nghĩa:

- Đảm bảo máy khác hoặc lần cài lại dependency vẫn lấy đúng package version `0.0.9`.
- Tránh `package.json` và lockfile lệch nhau.

#### `ios/Podfile.lock`

Đã thay đổi theo kết quả pod resolution:

- `EsignEasyca` không còn nằm trong danh sách pods/dependencies.
- `hdb-core-service` chuyển từ `1.0.156-uat.1` sang `1.0.156-uat.2`.
- Checksum của `CodePush`, `hdb-core-service`, `RCT-Folly`, và `PODFILE CHECKSUM` được cập nhật.

Ý nghĩa:

- Xác nhận CocoaPods đã nhận cấu hình mới.
- Đây là file bằng chứng cho thấy autolink/pod dependency đã thay đổi sau fix.

### Cách đọc lại nhanh khi gặp lỗi tương tự

1. Đọc `react-native.config.js` trước để xem có SDK nào bị tắt autolink vì chỉ chạy device không.
2. Đọc `ios/Podfile` để xem framework nào bị loại khỏi simulator embed phase.
3. Đọc `ios/hdb_host/Info.plist` để kiểm tra capability/architecture ràng buộc app install.
4. Đọc `package.json`, `yarn.lock`, và `ios/Podfile.lock` để xác nhận dependency version và pod resolution.

## Cách kiểm tra đã thấy trong project

- `SDKCA.framework`, `bshield.framework`, `AuthenticationIDSDK.framework`, `TrueIDFaceDetectSDK.framework` đều là Mach-O arm64 binary.
- `Pods-hdb_host-frameworks.sh` hiện có guard `if [[ "${PLATFORM_NAME}" != "iphonesimulator" ]]` cho 3 framework của `HDBLibs`; nếu là simulator thì remove framework khỏi app bundle.

## Trạng thái

completed

## Bài học

- Khi app iOS React Native crash/fail trước khi JS load trên simulator, cần kiểm tra native vendored frameworks trước, đặc biệt các SDK ngân hàng/eKYC/eSign có binary chỉ dành cho device thật.
- Với SDK chỉ hỗ trợ device, không để autolink/embed vào simulator target.
- Nếu Podfile phải patch generated CocoaPods scripts, cần patch cả `Pods-hdb_host-frameworks.sh` và các `*.xcfilelist`; nếu chỉ patch script mà file list vẫn giữ framework, Xcode có thể tiếp tục cố xử lý framework đó.
- Khi cập nhật lại lỗi trong WikiLLM từ project ngoài, cần phân biệt command dùng để điều tra hiện tại với command từng dùng để fix. Lần này chưa có lịch sử command fix cũ được xác nhận nên chưa ghi vào `terminal/fixbug.md`.
