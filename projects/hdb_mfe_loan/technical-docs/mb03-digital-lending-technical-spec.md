# Technical Digest - MB03 Digital Lending

Nguồn: `documents/MB03 - Phieu_Dac_Ta_Ky_Thuat_Digital_Lending_EDIT.docx`

Ngày nạp vào WikiLLM: 2026-06-08

Scope: tài liệu kỹ thuật Digital Lending cho webapp vay cầm cố STK, gateway, ESB, FPT eSign, JWT/AES flow. Phần tất toán chỉ ghi nhận, chưa phải scope chính hiện tại.

## Mô hình hệ thống

- WebApp: webapp nhúng trong Mobile Banking hiện tại, dùng cho vay cầm cố sổ tiết kiệm.
- WebApp Backend: tiếp nhận request từ client, xử lý business logic và gọi các lớp phía trong.
- Digital Services: xử lý business logic và gọi API ESB.
- ESB: trục tích hợp, public API cho Digital Services để xử lý nghiệp vụ core banking.
- ODBS: lưu dữ liệu giao dịch online.
- DWH: lưu trữ/archive dữ liệu phục vụ BI.

## Chức năng chính

- Đăng ký vay bảo đảm bằng sổ tiết kiệm trên Mobile Banking.
- Đăng ký tất toán khoản vay trên Mobile Banking.
- Quản lý khoản vay cầm cố STK online và tại quầy qua Webportal.

## Flow đăng ký vay theo technical spec

1. MB mở WebView load webapp vay cầm cố STK.
2. WebApp hiển thị điều khoản điều kiện, KH xác nhận và bấm tiếp tục.
3. WebApp gọi `getListDepositAccounts` đến GWDL.
4. GWDL gọi ESB để truy vấn danh sách STK trên CBS.
5. WebApp hiển thị danh sách STK và KH chọn STK.
6. WebApp gọi `GetInitData` / `getInitLoanProposal` để lấy dữ liệu khởi tạo:
   - Thông tin KH.
   - Danh sách tài khoản thanh toán.
   - Thông tin lãi dự thu.
   - Danh sách chi nhánh.
   - Cấu hình sản phẩm.
7. GWDL gọi các API ESB tương ứng:
   - `getCustomerInfo`
   - `getListCurrentAccounts`
   - `getProductCatalog`
   - `getListBranch`
8. KH nhập thông tin khoản vay: tài khoản giải ngân/thu nợ, số tiền vay, email, chi nhánh/PGD.
9. WebApp gọi `generateContract` để tạo PDF hợp đồng.
10. WebApp gọi `initLoanProposal` để khởi tạo giao dịch.
11. GWDL gọi FPT eSign:
   - `prepareCertificateForSignCloud`
   - `prepareFileForSignCloud`
12. FPT gửi OTP ký hợp đồng.
13. WebApp hiển thị màn hình nhập OTP theo bản web.
14. WebApp gọi `submitLoanProposal` kèm OTP.
15. GWDL gọi FPT `authorizeCounterSigningForSignCloud` để xác thực OTP và lấy file PDF đã ký.
16. GWDL gọi ESB `submitMortgageLoan` để tạo hồ sơ vay và giải ngân.
17. WebApp hiển thị kết quả thành công hoặc lỗi.

## API Digital Lending Gateway

Các API trong tài liệu đều là `POST`, JSON:

| API | Mục đích | Ghi chú |
| --- | --- | --- |
| `getListDepositAccounts` | Truy vấn danh sách STK của KH | Input chính: `clientNo`; output có danh sách STK, số tiền vay tối đa, trạng thái ký tại quầy. |
| `getInitInfo` / `getInitLoanProposal` | Truy vấn thông tin khởi tạo | Postman dùng tên `getInitLoanProposal`; technical spec ghi `getInitInfo`. |
| `generateContract` | Generate hợp đồng vay cầm cố STK | Output chính: `pdfBase64`. |
| `initLoanProposal` | Khởi tạo giao dịch vay cầm cố STK | Output chính: `agreementUUID`. |
| `submitLoanProposal` | Xác nhận giao dịch vay cầm cố STK | Input có `otp`; gọi FPT ký số và ESB tạo hồ sơ vay. |
| `resendOTP` | Gửi lại OTP | Input chính: `agreementUUID`. |

## API ESB được nhắc trong spec

| API | Mục đích |
| --- | --- |
| `getCustomerInfo` | Truy vấn thông tin KH theo CIF. |
| `getListDepositAccounts` | Truy vấn danh sách STK. |
| `getListCurrentAccounts` | Truy vấn danh sách tài khoản thanh toán. |
| `getListBranch` | Truy vấn danh sách chi nhánh. |
| `getProductCatalog` | Truy vấn cấu hình sản phẩm vay cầm cố STK. |
| `submitLoanProposal` | Mở khoản vay cầm cố STK. |
| `getListLoans` | Truy vấn danh sách khoản vay. |
| `getLoanDetail` | Truy vấn chi tiết khoản vay. |
| `getDigitalContract` | Truy vấn hợp đồng điện tử đã được ký bởi HDBank. |
| `getNextWorkingDate` | Truy vấn ngày làm việc tiếp theo. |
| `submitPayoffProposal` | Tất toán khoản vay cầm cố STK. |

## Envelope request chung

Các API gateway dùng envelope chung:

- `requestId`
- `channel`
- `ipRequest`
- `language`
- `deviceInfo`
  - `deviceId`
  - `deviceName`
  - `os`
  - `osVersion`
- `data`

## Field chính theo API

### `getListDepositAccounts`

Input chính:

- `clientNo`

Output account fields đáng chú ý:

- `accountNo`
- `balance`
- `maturityBalance`
- `maturityDate`
- `ccy`
- `collatCode`
- `paymentAcct`
- `intRate`
- `depositAcctAccruedAmt`: số tiền vay tối đa.
- `interestAtm`: số tiền lãi, theo spelling trong spec.
- `hasDeposit`: đã bị thế chấp hay chưa.
- `hasRestraint`: đã bị phong tỏa hay chưa.
- `openMethod`
- `minLoanAmount`
- `availableOutstanding`: số tiền vay khả dụng theo TT06.
- `isSigned`: đã ký tại quầy hay chưa.

### `getInitInfo` / `getInitLoanProposal`

Input:

- `clientNo`
- `userId`
- `depositAccounts`

Output đáng chú ý:

- `intRate`
- `disbursementDate`
- `maturityDate`
- `depositAcctAccruedAmt`
- `customerInfo`
- `product`
- `interestConfigs`
- `branches`

### `generateContract`

Input đáng chú ý:

- `loanAmt`
- `intRate`
- `maturityDate`
- `priPayment`
- `loanPurpose`
- `clientNo`
- `loanSubType`
- `branch`
- `paymentAcct`
- `email`
- `phoneNo`
- `proposalID`
- `listCollaterals`

Output:

- `pdfBase64`

### `initLoanProposal`

Input tương tự `generateContract`.

Output:

- `agreementUUID`

### `submitLoanProposal`

Input tương tự `initLoanProposal`, bổ sung:

- `otp`
- `agreementUUID`
- `billCode`
- `authenticationType`

Output:

- `resultCode`
- `resultMessage`

### `resendOTP`

Input:

- `agreementUUID`

Output:

- `agreementUUID`

## Security flow

- MB xác thực tài khoản KH với BE Mobile.
- BE Mobile sinh JWT qua IAM Gateway.
- MB mở WebView URL dạng `https://mobile.hdbank.com.vn/digitallending?token=[jwt_token]`.
- WebApp mã hóa request body bằng AES256.
- WebApp gửi request kèm JWT trong header `Authorization: Bearer <token>`.
- DLS Gateway giải mã body bằng AES256.
- DLS Gateway kiểm tra chuỗi request: ký tự đặc biệt, số lần request, độ dài chuỗi request, v.v.
- DLB Gateway xác thực JWT với IAM Gateway.
- Nếu JWT hợp lệ, DLB Gateway gọi ESB.

## Lưu ý bảo mật quan trọng

- Tài liệu kỹ thuật mô tả WebApp mã hóa body bằng AES256.
- Task pentest đã ghi nhận lỗi lộ secret key encrypt body trên FE.
- Khi làm task liên quan webapp, không giả định key nằm ở FE là hợp lệ. Frontend là môi trường public; secret phải được đưa về backend/service tin cậy hoặc dùng flow không cần secret lâu dài ở FE.

## Điểm cần kiểm tra khi làm task

- Spec gọi API khởi tạo là `getInitInfo`, Postman/source có thể dùng `getInitLoanProposal`.
- Bản web có màn hình OTP riêng; bản React Native hiện tại dùng popup/bottom sheet OTP.
- `isSigned` từ danh sách STK là field quan trọng để phân luồng đã ký/chưa ký GĐNVV.
- `depositAcctAccruedAmt`, `availableOutstanding`, `minLoanAmount`, `productCode`, `loanSubType` là các field cần ưu tiên khi debug rule số tiền vay/hạn mức.
