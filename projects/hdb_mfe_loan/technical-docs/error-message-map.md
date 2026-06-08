# Error Message Map - Digital Lending

Nguồn: `documents/errorMessage.json`

Ngày nạp vào WikiLLM: 2026-06-08

Scope: mapping error code sang message hiển thị cho webapp/RN vay cầm cố STK. File nguồn là JSON dạng:

```text
ERROR_CODE -> Vietnamese display message
```

## Nhóm lỗi chính

### Auth / token / access

- `INVALID_TOKEN`
- `ACCESS_TOKEN_EXPIRED`
- `ACCESS_TOKEN_IS_EXPIRED`
- `ACCESS_TOKEN_NOT_ALLOWED`
- `EXPIRE_TOKEN`
- `TOKEN_IS_EXPIRED`
- `INVALID_ACCESS_TOKEN`
- `GENERATE_TOKEN_FAIL`
- `INVALID_SIGNATURE`
- `INVALID_SUBJECT`

Ý nghĩa: lỗi xác thực, hết hạn token, sai chữ ký, không có quyền truy cập.

### API / dữ liệu đầu vào

- `TIMEOUT`
- `REQUEST_TIMEOUT`
- `FORMAT_ERROR`
- `INVALID_PARAM`
- `INVALID_PARAMETER`
- `SYSTEM_ERROR`
- `SYSTEM_IN_EOD`

Ý nghĩa: request timeout, format/input không hợp lệ, hệ thống bảo trì/EOD.

### Danh sách STK / tài khoản / sản phẩm

- `LIST_DEPOSIT_ACCOUNTS_FAIL`
- `GET_CLIENT_INFO_FAIL`
- `GET_PRODUCT_CATALOG_FAIL`
- `GET_LIST_ACCT_FAIL`
- `GET_INFO_ACCT_TERM_FAIL`
- `DEPOSIT_ACCOUNTS_INVALID`
- `LIST_COLLATERALS_INVALID`
- `PAYMENT_ACCT_INVALID`
- `GET_LIST_BRANCHES_FAIL`

Ý nghĩa: lỗi lấy danh sách STK, thông tin KH, catalog sản phẩm, tài khoản thanh toán, chi nhánh, hoặc STK/tài khoản không hợp lệ.

### Khoản vay / hợp đồng / lãi

- `GENERATE_CONTRACT_FAIL`
- `SUBMIT_LOAN_PROPOSAL_FAIL`
- `LOAN_AMOUNT_INVALID`
- `INVALID_LOAN_AMOUNT`
- `INT_RATE_INVALID`
- `INT_AMOUNT_INVALID`
- `MATURITY_DATE_INVALID`
- `CONTRACT_NO_INVALID`
- `CONTRACT_SIGNED_FAIL`
- `LOAN_LIMIT_OUTSTANDING_FAIL`
- `LIMIT_REMAINING_ERROR`

Ý nghĩa: lỗi tạo hợp đồng, đăng ký khoản vay, số tiền vay/lãi/ngày đáo hạn/hạn mức.

### FPT eSign / OTP

| Code | Message |
| --- | --- |
| `PREPARE_CERTIFICATE_FAIL` | Lỗi trong quá trình xác thực chữ ký số. Quý khách vui lòng thực hiện lại hoặc liên hệ 19006060 |
| `PREPARE_FILE_FAIL` | Lỗi trong quá trình thực hiện ký chữ ký số. Quý khách vui lòng thực hiện lại hoặc liên hệ 19006060 |
| `CERTIFICATE_IS_EXPIRED` | Lỗi xác thực chữ ký số. Quý khách vui lòng thực hiện lại hoặc liên hệ 19006060 |
| `VERIFY_OTP_FAIL` | Lỗi trong quá trình xác thực OTP. Quý khách vui lòng thực hiện lại hoặc liên hệ 19006060 |
| `OTP_IS_EXPIRED` | Mật khẩu giao dịch đã hết hạn. Quý khách vui lòng thực hiện lại giao dịch |
| `OTP_IS_INVALID` | Mật khẩu giao dịch không đúng. Quý khách vui lòng nhập lại |
| `RESEND_OTP_FAIL` | Gửi lại OTP không thành công. Quý khách vui lòng thực hiện lại |
| `REQUEST_OTP_FAIL` | Lỗi trong quá trình gửi OTP. Quý khách vui lòng thực hiện lại hoặc liên hệ 19006060 |
| `REQUEST_OTP_IS_OVER_LIMIT_PER_5_MIN` | Số lượt yêu cầu gửi lại OTP vượt quá quy định trong 5 phút |
| `AUTHORIZATION_IS_BLOCKED` | Lỗi xác thực chữ ký số. Quý khách vui lòng thực hiện lại hoặc liên hệ 19006060 |

Ghi chú:

- File có message giới hạn resend OTP theo chu kỳ 5 phút.
- File chưa nói rõ số lần resend tối đa hoặc số lần nhập sai tối đa.

### Điều kiện khách hàng

- `CLIENT_AGE_INVALID`
- `CLIENT_PHONE_NUMBER_INVALID`
- `CLIENT_IDENTITY_INVALID`
- `CLIENT_TYPE_INVALID`
- `PERSONAL_ID_TYPE_INVALID`

Ý nghĩa: lỗi điều kiện KH: tuổi, số điện thoại, GTTT, loại KH, loại giấy tờ.

### Tất toán / khoản vay đang có

- `GET_CHARGE_FEE_FAIL`
- `FEE_INVALID`
- `LOAN_NO_INVALID`
- `LOAN_SETTLE_INVALID`
- `PAYMENT_METHOD_INVALID`
- `BALANCE_ACCOUNT_NOT_ENOUGH`
- `LOAN_DETAIL_IS_UPDATING`

Ý nghĩa: lỗi tất toán/phí/tài khoản/khoản vay. Hiện chưa phải scope chính nếu người dùng chỉ làm đăng ký vay cầm cố STK.

## Cách dùng khi làm task

- Khi BE trả `resultCode`, tra `documents/errorMessage.json` trước.
- Nếu code không có trong file, fallback theo rule UI/source hiện tại.
- Với task React Native, cần kiểm tra message hiển thị có đúng design mobile không.
- Với task webapp, endpoint group đã được người dùng confirm là `/api-dl/*`.
