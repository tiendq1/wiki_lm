# BRD Digest - Vay cầm cố sổ tiết kiệm online

## Nguồn

- File gốc: `documents/BRD NANG HM VAY CC STK ONLINE (HDB VNP).pdf`
- Tên tài liệu: BRD - Đặc tả yêu cầu nâng hạn mức vay cầm cố sổ tiết kiệm online trên HDB App.
- Version: 1.0.
- Loại bản: BRD cho bản web/HDB App cũ.

## Phạm vi nạp vào WikiLLM

- Chỉ nạp phần đăng ký vay cầm cố sổ tiết kiệm.
- Không nạp phần tất toán khoản vay vì hiện tại người dùng chưa làm phần này.

## Context hiện tại

- Công việc chính hiện tại của người dùng: làm tính năng vay cầm cố bằng sổ tiết kiệm.
- Bản cũ: làm bằng web React JS và nhúng vào WebView của native mobile.
- Bản mới: làm bằng React Native TypeScript, là miniapp cho tính năng vay, được view/host vào `hdb_host`.
- Host project: `/Users/atien/work/app/hdb_host`.

## Mục tiêu BRD

- Điều chỉnh sản phẩm vay cầm cố sổ tiết kiệm online trên HDB App.
- Bổ sung luồng nâng hạn mức vay tối đa 1 tỷ đồng/khoản vay trên HDB App.
- Đối tượng: khách hàng cá nhân vay cầm cố sổ tiết kiệm online trên HDB App.

## Luồng quyết định hạn mức

### Kiểm tra khách hàng đã ký GĐNVV

- HDB App gọi API kiểm tra trạng thái ký GĐNVV từ Cus360.
- Cus360 kiểm tra và trả kết quả cho HDB App.

### Trường hợp chưa ký GĐNVV

- Khách hàng đi vào hành trình vay tối đa 100 triệu đồng/khách hàng.
- Luồng hiện tại: Product Code `001`.
- Loan subtype: `341`.
- Áp dụng hạn mức theo TT06.

### Trường hợp đã ký GĐNVV

- Khách hàng đi vào hành trình vay tối đa 1 tỷ đồng/khoản vay.
- Product Code mới: `007`.
- Loan subtype mới: `741`.
- Không áp dụng hạn mức TT06.
- Trình tự xử lý tương tự loan subtype `341`.

## Các bước/màn hình đăng ký

### 1. Chọn sản phẩm vay / CVP

- Màn hình chọn sản phẩm vay: không điều chỉnh.
- Thay màn hình T&C bằng màn hình CVP theo thiết kế.
- Button `Đăng ký ngay` luôn enable.
- Khi khách hàng chọn đăng ký, app kiểm tra điều kiện như màn hình T&C hiện tại.
- Nếu thỏa điều kiện: chuyển sang màn hình danh sách sổ tiết kiệm.
- Nếu không thỏa: hiển thị thông báo tương ứng, khách hàng đóng thông báo và ở lại màn hình CVP.

Điều kiện kiểm tra gồm các nhóm hiện tại:

- Người nước ngoài.
- Đối tượng hạn chế cấp tín dụng.
- Trạng thái KYC.
- Giấy tờ tùy thân.
- Số điện thoại.
- Độ tuổi.

### 2. FAQ trên CVP

Các ý chính cần giữ:

- Vay cầm cố STK giúp khách hàng vay vốn tiêu dùng nhanh mà không cần tất toán tiền gửi tiết kiệm trước hạn.
- Sổ online có thể hoàn tất vay trên app nếu nằm trong hạn mức/điều kiện phù hợp.
- Sổ online vay trên 100 triệu hoặc sổ mở tại quầy cần làm thủ tục một lần tại quầy trước, sau đó chủ động giải ngân trên app.
- Hạn mức:
  - Sổ online chưa ký GĐNVV: tối đa 100 triệu đồng/khách hàng.
  - Sổ online đã ký GĐNVV tại quầy: tối đa 1 tỷ đồng/khoản vay.
  - Sổ tại quầy đã ký thủ tục: tối đa 3 tỷ đồng/khoản vay.

### 3. Danh sách sổ tiết kiệm

Điều kiện load STK:

- Acctype: `ON0`, `ONL`.
- Loại tiền: VND.
- Sổ không đồng sở hữu.
- `ownership_type = SG`.
- Sổ không bị phong tỏa.
- Sổ không đang bảo đảm vay.

Điều chỉnh chính:

- Nhận diện khách hàng đã ký GĐNVV hay chưa.
- Nếu chưa ký GĐNVV: điều hướng vào luồng Product Code `001`, tối đa 100 triệu đồng/khách hàng.
- Nếu đã ký GĐNVV: điều hướng vào luồng Product Code `007`, tối đa 1 tỷ đồng/khoản vay.
- Đổi text `Giá trị sổ tiết kiệm` thành `Số tiền vay tối đa`.
- `Số tiền vay tối đa` lấy theo field số tiền có thể vay tối đa.
- So sánh Product Min với số tiền vay tối đa của STK để enable/disable STK:
  - Nếu số tiền vay tối đa >= Product Min: cho chọn STK.
  - Nếu số tiền vay tối đa < Product Min: làm mờ STK.
- Giữ rule hiển thị STK hiện tại và bổ sung rule Product Min.

Ghi chú `Cách tính số tiền vay tối đa`:

- Tiền gửi lĩnh lãi cuối kỳ:
  - Số tiền vay tối đa = Số dư tiền gửi + Lãi tiền gửi dự kiến - Chi phí liên quan tiền gửi - Lãi vay dự kiến khi đến hạn - Phí liên quan khoản vay.
- Tiền gửi lĩnh lãi định kỳ/đầu kỳ:
  - Số tiền vay tối đa = Số dư tiền gửi - Chi phí liên quan tiền gửi - Lãi vay dự kiến khi đến hạn - Phí liên quan khoản vay.
- Số tiền vay tối đa không vượt quá số tiền gốc của sổ tiết kiệm.
- Chỉ hiển thị với sổ online và khách hàng chưa ký GĐNVV:
  - Số tiền vay tối đa không vượt quá hạn mức vay online khả dụng theo TT06.
  - `<xxx>` là hạn mức TT06 khả dụng do BE trả về.

Màn hình không có STK:

- Điều chỉnh theo thiết kế.
- Button `Quay lại` điều hướng khách hàng về màn hình CVP.

### 4. Nhập số tiền vay

- Số tiền cần vay mặc định bằng số tiền có thể vay tối đa.
- Cho phép khách hàng điều chỉnh.
- Hiển thị lưu ý:
  - Tối thiểu `<Số tiền vay tối thiểu>` VND.
  - Tối đa `<Số tiền có thể vay vốn>` VND.
- Nếu khách hàng nhập ngoài khoảng:
  - Bấm tiếp tục thì hiện cảnh báo `Số tiền vay phải nằm trong khoảng từ ... đến ...`.
  - Disable button `Tiếp tục`.
  - Không cho đi tiếp.
- Nếu nhập đúng: chuyển màn hình xác nhận.

Ghi chú cần hiển thị:

- `Để vay cầm cố Sổ mở Online lên đến 1 tỷ đồng/khoản vay, Quý khách vui lòng đến CN/PGD gần nhất để ký Giấy đề nghị vay vốn`.
- Điều kiện hiển thị: khách hàng vay cầm cố bằng STK online và chưa ký GĐNVV tại quầy.

Tài khoản giải ngân:

- Mặc định là tài khoản nhận lãi của STK theo rule hiện tại.
- Nếu khách hàng không có TKTT thỏa điều kiện:
  - Để trống thông tin.
  - Cho phép khách hàng chọn TKTT khác.
- Dùng chung danh sách TKTT với luồng Product Code `002` - sổ tại quầy.

### 5. Xác nhận thông tin khoản vay

- Hiển thị thông tin theo thiết kế.
- Tài khoản nhận giải ngân và trích nợ tự động: thông tin khách hàng đã chọn, không cho điều chỉnh.
- Đơn vị quản lý:
  - Như hiện tại.
  - Sổ tại quầy: không cho điều chỉnh.
  - Sổ online: cho phép khách hàng chọn lại đơn vị khác qua popup.
- Email:
  - Bật popup cho khách hàng điều chỉnh.
  - Rule validate như hiện tại.
- Các field khác: như hiện tại.

Sau khi khách hàng điền thông tin và xác nhận:

- MB hiển thị popup Điều khoản điều kiện và Hợp đồng tín dụng.
- Điều khoản, điều kiện: bật popup file T&C để khách hàng đọc.
- Hợp đồng vay cầm cố STK: gen file hợp đồng và bật popup để khách hàng đọc.
- Điều khoản, điều kiện Chứng từ số: dùng file chung.

### 6. Ký số

- Thêm màn hình xác thực ký số FPT.
- Khách hàng có thể bấm `Lấy mã OTP` để gửi lại mã OTP mới.
- Khi gửi lại OTP:
  - MB xóa OTP cũ.
  - Gửi mã mới.
  - Hiển thị cảnh báo tương ứng:
    - `Đã gửi lại Mã OTP`.
    - `Không thể lấy mã OTP. Vui lòng thử lại`.
- Rule xác thực FPT eSign như hiện tại.

### 7. Đăng ký vay thành công

Điều chỉnh màn hình gửi yêu cầu vay thành công:

- Text: `GỬI YÊU CẦU THÀNH CÔNG`.
- Hiển thị thời gian thực hiện giao dịch: `<hh:mm - dd/mm/yyyy>`.
- Hiển thị số tiền vay vốn và đơn vị.
- Hiển thị nội dung:
  - `Vay cầm cố bằng Sổ tiết kiệm <Số STK>, lãi suất vay <lãi suất vay> %/năm.`

Button `Chi tiết giao dịch`:

- Mặc định ở chế độ thu gọn.
- Khi khách hàng chọn, hiển thị:
  - Số yêu cầu.
  - Họ tên khách hàng.
  - Ngày hiệu lực.
  - Ngày đáo hạn.
  - Tài khoản giải ngân.
  - Đơn vị quản lý.
  - Email nhận hợp đồng.

Điều hướng:

- Button `Giao dịch khác`: về danh sách sổ tiết kiệm.
- Icon Home: về Home screen.
- Đánh giá trải nghiệm: dùng chức năng hiện có.

## Công thức tính số tiền có thể vay tối đa

Áp dụng cho:

- Sổ online chưa ký GĐNVV: Product Code `001`.
- Sổ tại quầy: Product Code `002`.
- Sổ online đã ký GĐNVV: Product Code `007`.

Các field chính:

- Giá trị của STK: trị giá STK do BE trả về.
- Lãi suất vay: lãi suất STK + biên độ, không thấp hơn lãi suất vay tối thiểu của sản phẩm.
- Thời hạn vay: số ngày vay tối đa của sổ được chọn.
- Ngày đến hạn khoản vay tối đa không vượt quá 36 tháng.

Công thức:

```text
Số tiền có thể vay tối đa =
MIN [
  Giá trị STK / (1 + Lãi suất vay / 365 * Thời gian),
  Mệnh giá,
  Room khả dụng,
  Số tiền vay tối đa theo sản phẩm,
  HMTT06 nếu có
]
```

Làm tròn:

- Làm tròn xuống đến hàng trăm nghìn.

Giới hạn theo sản phẩm:

- Sổ online chưa ký GĐNVV: tối đa 100 triệu đồng/khách hàng.
- Sổ tại quầy: tối đa 3 tỷ đồng/khoản vay.
- Sổ online đã ký GĐNVV: tối đa 1 tỷ đồng/khoản vay.
- Hạn mức TT06 chỉ áp dụng cho sổ online chưa ký GĐNVV.

## Integration / hệ thống liên quan

- HDB App / MB.
- Cus360: kiểm tra trạng thái khách hàng đã ký GĐNVV.
- BE/JOB quản lý room tín dụng theo sản phẩm.
- ECRM / Job giải ngân.
- SYMBOL:
  - Khai báo thêm LoanSubType `741`.
  - LoanSubType `741` có tính chất theo LoanSubType `341`.
- Portal quản lý hạn mức vay online:
  - Product `341`: ghi nhận hạn mức vay online như hiện tại.
  - Product `741`: không ghi nhận vào hạn mức vay online của khách hàng.

## Không nạp hiện tại

- Phần tất toán khoản vay.
- Tất toán trên MB.
- Tất toán tại quầy.
- Tất toán tự động khi đến hạn.
- Cập nhật hồi room sau tất toán.

## Ghi chú cho agent

- Khi người dùng hỏi về BRD này, ưu tiên phạm vi đăng ký vay cầm cố STK.
- Nếu câu hỏi đụng đến tất toán, nhắc rằng phần đó hiện chưa nạp vì người dùng chưa làm.
- Khi làm task React Native mới, mapping logic web cũ sang RN TypeScript/miniapp nhưng vẫn cần kiểm tra source thực tế trong `hdb_mfe_loan` và host integration trong `hdb_host`.
