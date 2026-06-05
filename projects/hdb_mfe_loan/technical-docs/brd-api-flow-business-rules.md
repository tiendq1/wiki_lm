# BRD Extract - API, Flow, Business Rules

Nguồn: `documents/BRD NANG HM VAY CC STK ONLINE (HDB VNP).pdf`

Scope: chỉ phần đăng ký vay cầm cố sổ tiết kiệm. Phần tất toán chỉ ghi nhận là out-of-scope hiện tại.

## API / integration được BRD nhắc đến

BRD không có API spec chi tiết kiểu endpoint/request/response. Các điểm integration có thể xác định:

- HDB App gọi API Cus360 để kiểm tra trạng thái KH đã ký ĐNVV/GĐNVV tại quầy hay chưa.
- Cus360 nhận yêu cầu, kiểm tra thông tin KH ký ĐNVV, rồi trả kết quả cho HDB App.
- Kết quả này quyết định luồng:
  - Chưa ký GĐNVV: Product Code `001`, LoanSubType `341`, áp dụng TT06, tối đa 100 triệu đồng/khách hàng.
  - Đã ký GĐNVV: Product Code `007`, LoanSubType `741`, không áp dụng TT06, tối đa 1 tỷ đồng/khoản vay.
- JOB BE quản lý room tín dụng theo sản phẩm như luồng hiện tại; luồng `741` ghi nhận chung room với sản phẩm `341`.
- JOB giải ngân tiếp nhận và khởi tạo khoản vay theo yêu cầu ECRM.
- SYMBOL cần khai báo thêm LoanSubType `741`, tính chất và trình tự như LoanSubType `341`.
- Portal quản lý hạn mức vay online:
  - Với `341`: ghi nhận hạn mức vay online của KH như hiện tại.
  - Với `741`: không ghi nhận vào hạn mức vay online của KH.
- BE trả một số field/rule đầu vào cho app:
  - Trị giá STK.
  - Số tiền có thể vay tối đa.
  - Hạn mức TT06 khả dụng nếu có.
  - Room khả dụng.
  - Product Min.
  - Lãi suất vay, ngày đến hạn, thời hạn vay.

## Flow vay hiện tại theo BRD

1. KH mở STK online hoặc tại quầy.
2. KH đăng ký vay cầm cố STK trên HDB App.
3. HDB App gọi API Cus360 kiểm tra trạng thái KH đã ký ĐNVV/GĐNVV.
4. Cus360 kiểm tra và trả kết quả.
5. Nếu KH chưa ký GĐNVV:
   - Đi luồng Product Code `001`.
   - LoanSubType `341`.
   - Hạn mức tối đa 100 triệu đồng/khách hàng.
   - Áp dụng hạn mức TT06.
6. Nếu KH đã ký GĐNVV:
   - Đi luồng Product Code `007`.
   - LoanSubType `741`.
   - Hạn mức tối đa 1 tỷ đồng/khoản vay.
   - Không áp dụng hạn mức TT06.
7. App thực hiện hành trình vay cầm cố STK hiện tại:
   - CVP/chọn sản phẩm.
   - Danh sách STK.
   - Nhập số tiền vay.
   - Xác nhận thông tin khoản vay.
   - Popup điều khoản điều kiện.
   - Popup hợp đồng vay cầm cố STK.
   - Ký số/OTP.
   - Kết quả đăng ký vay thành công.

## Rule nghiệp vụ chính

### Kiểm tra điều kiện trước khi vào danh sách STK

- Người nước ngoài.
- Đối tượng hạn chế cấp tín dụng.
- Trạng thái KYC.
- Giấy tờ tùy thân.
- Số điện thoại.
- Độ tuổi.
- Nếu thỏa: chuyển danh sách STK.
- Nếu không thỏa: hiển thị thông báo tương ứng, KH đóng thông báo và ở lại CVP.

### Điều kiện load STK

- Acctype: `ON0`, `ONL`.
- Loại tiền: VND.
- Sổ không đồng sở hữu.
- `ownership_type = SG`.
- Sổ không bị phong tỏa.
- Sổ không đang bảo đảm vay.

### Enable/disable STK

- App lấy `Số tiền vay tối đa` theo field `Số tiền có thể vay tối đa`.
- So sánh `Product Min` với `Số tiền vay tối đa` của STK:
  - Nếu `Số tiền vay tối đa >= Product Min`: cho chọn STK.
  - Nếu `Số tiền vay tối đa < Product Min`: làm mờ STK.

### Công thức số tiền vay tối đa

- Tiền gửi lĩnh lãi cuối kỳ:
  - Số tiền vay tối đa = Số dư tiền gửi + Lãi tiền gửi dự kiến - Chi phí liên quan tiền gửi - Lãi vay dự kiến khi đến hạn - Phí liên quan khoản vay.
- Tiền gửi lĩnh lãi định kỳ/đầu kỳ:
  - Số tiền vay tối đa = Số dư tiền gửi - Chi phí liên quan tiền gửi - Lãi vay dự kiến khi đến hạn - Phí liên quan khoản vay.
- Số tiền vay tối đa không vượt quá số tiền gốc của STK.
- Với sổ online và KH chưa ký GĐNVV: số tiền vay tối đa không vượt quá hạn mức vay online khả dụng theo TT06.
- Bảng BRD mô tả công thức tổng quát:
  - `MIN[(Giá trị STK)/(1 + Lãi suất vay/365 * Thời gian), Mệnh giá, Room khả dụng, Số tiền vay tối đa theo sản phẩm, HMTT06 nếu có]`
  - Làm tròn xuống đến trăm ngàn.

### Hạn mức theo loại sản phẩm

- Sổ online chưa ký GĐNVV: tối đa 100 triệu đồng/khách hàng.
- Sổ online đã ký GĐNVV tại quầy: tối đa 1 tỷ đồng/khoản vay.
- Sổ tại quầy đã ký thủ tục: tối đa 3 tỷ đồng/khoản vay.

### Nhập số tiền vay

- Số tiền cần vay mặc định bằng số tiền có thể vay tối đa.
- KH được điều chỉnh số tiền.
- Hiển thị min/max:
  - Tối thiểu `<Số tiền vay tối thiểu>` VND.
  - Tối đa `<Số tiền có thể vay vốn>` VND.
- Nếu nhập ngoài khoảng:
  - Hiện cảnh báo.
  - Disable button tiếp tục.
  - Không cho đi tiếp.
- Ghi chú nâng hạn mức 1 tỷ chỉ hiển thị khi KH vay bằng STK online và chưa ký GĐNVV tại quầy.

### Tài khoản giải ngân

- Mặc định là tài khoản nhận lãi của STK theo rule hiện tại.
- Nếu KH không có TKTT thỏa điều kiện:
  - Để trống thông tin.
  - Cho phép KH chọn TKTT khác.
- Dùng chung danh sách TKTT với luồng Product Code `002` - sổ tại quầy.

### Xác nhận thông tin

- Tài khoản nhận giải ngân và trích nợ tự động: lấy theo thông tin KH đã chọn, không cho điều chỉnh.
- Đơn vị quản lý:
  - Sổ tại quầy: không cho điều chỉnh.
  - Sổ online: cho phép chọn lại qua popup.
- Email: cho điều chỉnh qua popup, validate như hiện tại.
- Điều khoản điều kiện: bật popup file T&C.
- Hợp đồng vay cầm cố STK: gen file hợp đồng và bật popup cho KH đọc.
- Điều khoản điều kiện chứng từ số: dùng file chung.

### OTP / ký số

- BRD/web ReactJS: có màn hình xác thực ký số FPT / màn hình nhập OTP.
- Mobile React Native hiện tại: chỉ có popup/bottom sheet OTP.
- KH có thể bấm lấy lại OTP; app xóa OTP cũ, gửi mã mới, hiển thị message thành công/thất bại.
- Rule xác thực FPT eSign dùng như hiện tại, nhưng BRD chưa ghi chi tiết rule kỹ thuật.

## Out-of-scope hiện tại

- Tất toán khoản vay có trong BRD nhưng người dùng nói hiện chưa làm.
- Không lấy phần tất toán làm scope mặc định khi nhận task vay cầm cố STK, trừ khi người dùng yêu cầu.

## Chưa rõ / cần tài liệu thêm

- API spec chi tiết cho Cus360 check signed status.
- Field mapping chi tiết từ BE sang UI/source mobile.
- Prototype/design React Native mới.
- Tài liệu integration `hdb_host`.
- Rule kỹ thuật FPT eSign/OTP.
