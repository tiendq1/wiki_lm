# Postman API Index - Webapp Digital Lending

Nguồn: `documents/Webapp Digital Lending - Pentest.postman_collection.json`

Ngày nạp vào WikiLLM: 2026-06-08

Collection name: `DL4`

Scope: index endpoint và field chính từ Postman collection. Không copy token/API key/client secret vào wiki.

## Cảnh báo bảo mật

- Collection có chứa token/API key/client secret ở một số request.
- Không paste raw header/body chứa secret vào tài liệu wiki, chat, log, hoặc report.
- Khi cần dùng collection để debug, đọc local file trực tiếp và redact trước khi chia sẻ.
- Điều này liên quan trực tiếp với task pentest: `tasks/web/2026-06-05-pentest-secret-key-encrypt-body-fe.md`.

## Nhóm API chính: Di-HDBank lending v1

Base path thường thấy:

- Local: `localhost:10000/api/lending/v1`
- UAT/STG: `{{di_hdbank_domain}}/api/lending/v1`

Header thường gặp:

- `GTW-Authorization`
- `Accept-Language`
- `X-Request-ID`
- `X-Device-ID`
- `X-CHANNEL`
- `Content-Type`

Không lưu giá trị token trong wiki.

| API | Method | Data keys chính |
| --- | --- | --- |
| `getClientInfo` | POST | thường rỗng hoặc theo token/session |
| `productStatus` | POST | product/status check |
| `getListDepositAccounts` | POST | `clientNo` |
| `getInitLoanProposal` | POST | `clientNo`, `depositAccounts` |
| `getListCurrentAccounts` | POST | thường theo token/session hoặc `clientNo` |
| `generateContract` | POST | `loanAmt`, `intRate`, `maturityDate`, `clientNo`, `branch`, `paymentAcct`, `email`, `phoneNo`, `intAmount`, `disbursementDate`, `listCollaterals` |
| `initLoanProposal` | POST | thông tin khoản vay, `contractNo`, `listCollaterals` |
| `submitLoanProposal` | POST | `otp`, `billCode`, `agreementUUID`, `authenticationType`, thông tin khoản vay, `loanSubType`, `contractNo`, `listCollaterals` |
| `resendOTP` | POST | `agreementUUID` |
| `getSignedContract` | POST | `loanNo`, `clientNo` |
| `registerLoanDemand` | POST | thông tin nhu cầu vay, KH, khoản vay, tài sản, repayment |
| `getListBranches` | POST | envelope request |
| `address-wards` / `address-districts` / `address-provinces` | POST | lookup địa chỉ |

## Nhóm archived API

Base cũ thường thấy:

- Local: `localhost:8082/api/v1`
- UAT: `https://digital-lending-uat.hdbank.com.vn/api-dl`

Endpoint archived đáng chú ý:

- `getListDepositAccounts`
- `getClientInfo`
- `productStatus`
- `getInitLoanProposal`
- `generateContract`
- `initLoanProposal`
- `submitLoanProposal`
- `resendOTP`
- `getListLoans`
- `getLoanDetails`
- `getListCurrentAccounts`
- `initSettleLoan`
- `submitSettleLoan`
- `getSignedContract`
- `getChargeFee`
- `getLoanHistory`
- `submitRating`

Ghi chú: nhóm này có thể là flow/webapp cũ, dùng để đối chiếu khi gặp legacy code.

## Nhóm ESB / Digital Core trong Postman

Các endpoint hoặc domain được Postman nhắc đến:

- `/oauth2/token`
- `/deposit/inquiryAcctInfo`
- `/deposit/inquirySavingAcctInfo`
- `/deposit/createDepAcct`
- `/limit/inquiryLimitDigitalInfo`
- `/limit/createLimitDigitalInfo`
- `/collateral/inquiryCollateralOnline`
- `/collateral/inquiryCollateralInfo`
- `/operation/inquiryPortalInfo`
- `/customer/inquiryCustInfo`
- `/customer/fetchStaticLocaltionInfo`
- `/customer/fetchStaticPersonInfo`
- `getLoanDetails_wpV1`

Header nhạy cảm thường gặp:

- `X-IBM-Client-Id`
- `X-IBM-Client-Secret`
- `Authorization`

Không ghi giá trị thật vào wiki.

## Nhóm DTC services

Các endpoint đáng chú ý:

- `generateTemplate`
- `cs-contract-api/resendOTP`
- `cs-contract-api/initContract`
- `erequest/api/v1/getRemainingLimit`
- `360app/api/transactioninternal/mobile/loan-identification-status`
- `360app/api/transactioninternal-stg/mobile/loan-identification-status`

Ý nghĩa:

- `generateTemplate`: gen template/hợp đồng.
- `resendOTP`: gửi lại OTP hợp đồng/ký số.
- `initContract`: khởi tạo hợp đồng.
- `getRemainingLimit`: truy vấn hạn mức còn lại.
- `loan-identification-status`: kiểm tra trạng thái định danh/ký GĐNVV từ 360 app/Cus360 context.

## Mapping nhanh với flow vay cầm cố STK

| Flow | API Postman khả năng liên quan |
| --- | --- |
| Vào module / lấy thông tin KH | `getClientInfo` |
| Kiểm tra product/status | `productStatus` |
| Danh sách STK | `getListDepositAccounts` |
| Lấy dữ liệu màn nhập khoản vay | `getInitLoanProposal`, `getListCurrentAccounts`, `getListBranches` |
| Gen hợp đồng | `generateContract`, `generateTemplate` |
| Khởi tạo ký số/giao dịch | `initLoanProposal`, `initContract` |
| Nhập/gửi lại OTP | `resendOTP` |
| Xác nhận vay | `submitLoanProposal` |
| Lấy hợp đồng đã ký | `getSignedContract` |
| Truy vấn khoản vay | `getListLoans`, `getLoanDetails` |
| Tất toán | `initSettleLoan`, `submitSettleLoan`, `getChargeFee` |

## Điểm cần nhớ

- Postman có cả flow mới `/api/lending/v1/*` và flow archived `/api/v1/*` hoặc `/api-dl/*`.
- Khi debug source webapp `/Users/atien/work/web/lending_uat_2026`, cần xác định source đang dùng nhóm endpoint nào.
- Không đưa raw Postman collection lên nơi bên ngoài vì có secret/token/API key.
