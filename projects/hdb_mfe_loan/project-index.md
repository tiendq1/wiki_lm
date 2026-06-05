# Project Index - hdb_mfe_loan

## Tổng quan

- Loại: Mobile / React Native.
- Project name trong Wiki: `vay`.
- Source project: `hdb_mfe_loan`.
- Module chính người dùng đang làm: vay cầm cố sổ tiết kiệm.
- Wiki path: `/Users/atien/AI/WikiLLM/projects/hdb_mfe_loan`.
- Source path React Native miniapp: `/Users/atien/work/app/hdb_mfe_loan`.
- Source path webapp vay cầm cố STK: `/Users/atien/work/web/lending_uat_2026`.
- Host project: `hdb_host`.
- Host path: `/Users/atien/work/app/hdb_host`.

## Stack đọc từ source

- React Native: `0.77.0`.
- React: `18.3.1`.
- TypeScript: `5.7.2`.
- Navigation:
  - `@react-navigation/native`
  - `@react-navigation/native-stack`
  - `@react-navigation/bottom-tabs`
- State:
  - Redux Toolkit
  - React Redux
- Module federation/repack:
  - `@callstack/repack`
  - `@module-federation/enhanced`

## Commands từ `package.json`

Các command dưới đây chạy tại:

```text
/Users/atien/work/app/hdb_mfe_loan
```

### Chạy Metro

```sh
yarn start
```

- Script: `react-native start --port=8085 --reverse-port`.
- Dùng để chạy Metro ở port `8085`.

```sh
yarn start:standalone
```

- Script: `STANDALONE=true react-native start --port=8082 --reverse-port`.
- Dùng khi chạy standalone mode ở port `8082`.

### Chạy app

```sh
yarn ios
```

- Script: `react-native run-ios --no-packager`.
- Lưu ý: script không tự mở packager, nên cần chạy Metro riêng trước.

```sh
yarn android
```

- Script: `react-native run-android --active-arch-only --no-packager`.
- Lưu ý: script không tự mở packager, nên cần chạy Metro riêng trước.

### Test / lint

```sh
yarn test
```

- Script: `jest`.
- Dùng để chạy test.

```sh
yarn lint
```

- Script: `eslint .`.
- Dùng để chạy lint toàn project.

### Bundle

```sh
yarn bundle:ios
```

- Script: `react-native bundle --platform ios --entry-file index.ts --dev false`.

```sh
yarn bundle:android
```

- Script: `react-native bundle --platform android --entry-file index.ts --dev false`.

```sh
yarn bundle:ios:remote
```

- Script: `react-native bundle --platform ios --entry-file index.ts --dev false --webpackConfig './webpack.remote.mjs'`.

```sh
yarn bundle:android:remote
```

- Script: `react-native bundle --platform android --entry-file index.ts --dev false --webpackConfig './webpack.remote.mjs'`.

## Entry và navigation

### App entry

- `index.ts`
  - Register component từ `@src/App`.

### Navigator chính

- `src/presentation/navigation/MainNavigator.tsx`
  - Initial route: `products`.
  - Stack route liên quan vay cầm cố STK:
    - `collateral_cvp`
    - `collateral_accounts`
    - `collateral_amount`
    - `collateral_confirm`
    - `collateral_result`

### Host navigation bridge

- `src/presentation/navigation/miniAppNavigator.tsx`
  - Dùng `eventBusService.emit('open_app', ...)`.
  - Liên quan việc miniapp được host/view từ `hdb_host`.

## Module vay cầm cố sổ tiết kiệm

Source module:

```text
src/presentation/loan_collateral/
```

Các màn hình/source chính:

| Flow | Route | Source |
| --- | --- | --- |
| CVP / giới thiệu sản phẩm | `collateral_cvp` | `src/presentation/loan_collateral/collateral_CVP/CollateralCVPScreen.tsx` |
| Danh sách sổ tiết kiệm | `collateral_accounts` | `src/presentation/loan_collateral/collateral_accounts/CollateralAccountsScreen.tsx` |
| Nhập số tiền muốn vay | `collateral_amount` | `src/presentation/loan_collateral/collateral_amount/CollateralAmountScreen.tsx` |
| Xác nhận thông tin | `collateral_confirm` | `src/presentation/loan_collateral/collateral_confirm/CollateralConfirmScreen.tsx` |
| Kết quả | `collateral_result` | `src/presentation/loan_collateral/collateral_result/CollateralResultScreen.tsx` |

Source liên quan khác:

- `src/presentation/loan_collateral/collateral_periods/CollateralPeriodsScreen.tsx`
- `src/presentation/loan_collateral/collateral_accounts/components/NoDataView.tsx`
- `src/presentation/loan_collateral/collateral_accounts/components/BottomSheetLoanInfo.tsx`
- `src/presentation/loan_collateral/collateral_confirm/components/SectionInfoDetail.tsx`
- `src/presentation/loan_collateral/collateral_confirm/components/SectionCustomerInfo.tsx`
- `src/presentation/loan_collateral/collateral_confirm/components/SectionLoanInfo.tsx`

## Flow chính theo người dùng mô tả

1. Màn hình danh sách sổ tiết kiệm.
2. Màn hình nhập số tiền muốn vay.
3. Màn hình xác nhận thông tin.
4. Popup hợp đồng vay.
5. Popup điều khoản điều kiện.
6. Popup nhập OTP.
7. Màn hình kết quả.

## Đối chiếu với BRD đã nạp

BRD đã nạp:

```text
projects/hdb_mfe_loan/brd/vay-cam-co-stk-online-registration-web-brd.md
```

BRD có mô tả các phần liên quan:

- Danh sách sổ tiết kiệm.
- Nhập số tiền vay.
- Xác nhận thông tin khoản vay.
- Popup Điều khoản điều kiện.
- Popup Hợp đồng tín dụng / Hợp đồng vay cầm cố STK.
- Ký số OTP.
- Đăng ký vay thành công.

Điểm cần lưu ý:

- BRD/web ReactJS gọi phần OTP là `màn hình xác thực ký số FPT` / màn hình nhập OTP.
- Mobile React Native hiện tại không dùng màn hình OTP riêng; OTP là popup/bottom sheet.
- Khác biệt này do design web WebView cũ và design mobile React Native mới không giống nhau.
- Source/tracking mobile hiện tại thể hiện OTP là bottom sheet/popup:
  - `BTMSHEET_SOFT_OTP`
  - `BTMSHEET_SOFT_OTP_SUCCESS`
  - `BTMSHEET_SOFT_OTP_FAIL`
  - `BTMSHEET_OTP_RESEND`
- Vì vậy khi làm task React Native mới, dùng wording thực tế của mobile source/design: popup/bottom sheet nhập OTP.

## Tài liệu đã nạp

- BRD đăng ký vay cầm cố sổ tiết kiệm:
  - `brd/vay-cam-co-stk-online-registration-web-brd.md`
  - Scope: đăng ký vay cầm cố STK.
  - Không bao gồm tất toán.
- Technical digest từ BRD:
  - `technical-docs/brd-api-flow-business-rules.md`
  - Scope: API/integration được nhắc trong BRD, flow vay hiện tại, rule nghiệp vụ.

## Tài liệu chưa nạp / chưa có

- URD riêng cho bản React Native TypeScript.
- Technical docs riêng cho host integration với `hdb_host`.
- API docs chi tiết dạng endpoint/request/response cho module vay cầm cố STK.
- Thiết kế UI/prototype nếu có.
- Các điểm còn thiếu đang được gom ở `memory/open-question.md`.

## Task / lỗi liên quan

- Web/security:
  - Task: `tasks/web/2026-06-05-pentest-secret-key-encrypt-body-fe.md`
  - Error: `errors/security/2026-06-05-pentest-secret-key-encrypt-body-fe.md`
  - Jira: https://hdbank.atlassian.net/browse/ITSWS05-3117
  - Source path: `/Users/atien/work/web/lending_uat_2026`
  - Ghi chú: task thuộc webapp vay cầm cố sổ tiết kiệm.

## Ghi chú cho agent

- Đây là module ưu tiên hiện tại của người dùng.
- Khi nhận yêu cầu liên quan vay cầm cố STK, đọc file này trước, sau đó đọc BRD digest và source liên quan.
- Không coi việc cập nhật file này là task nếu người dùng chỉ đang nạp context/cải thiện Wiki.
