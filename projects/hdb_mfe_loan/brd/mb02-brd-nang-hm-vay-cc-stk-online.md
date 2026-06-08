# BRD Digest - MB02 Nâng hạn mức vay cầm cố STK online

Nguồn: `documents/MB02-BRD NANG HM VAY CC STK ONLINE (HDB VNP).docx`

Ngày nạp vào WikiLLM: 2026-06-08

Scope: vay cầm cố sổ tiết kiệm online. Ưu tiên phần đăng ký vay; phần tất toán chỉ ghi nhận là ngoài scope hiện tại nếu người dùng chưa yêu cầu.

## Tóm tắt chính

- HDB bổ sung luồng nâng hạn mức vay tối đa 1 tỷ đồng/khoản vay cho KH đã ký ĐNVV/GĐNVV tại quầy.
- HDB App gọi API Cus360 để kiểm tra trạng thái KH đã ký ĐNVV/GĐNVV.
- Nếu chưa ký GĐNVV:
  - Đi luồng hiện tại Product Code `001`.
  - LoanSubType `341`.
  - Tối đa 100 triệu đồng/khách hàng.
  - Áp dụng hạn mức TT06.
- Nếu đã ký GĐNVV:
  - Đi luồng Product Code `007`.
  - LoanSubType `741`.
  - Tối đa 1 tỷ đồng/khoản vay.
  - Không áp dụng hạn mức TT06.
  - Trình tự xử lý tương tự LoanSubType `341`.

## Flow đăng ký vay

1. KH mở STK online hoặc tại quầy.
2. KH đăng ký vay cầm cố STK trên HDB App.
3. HDB App kiểm tra trạng thái KH đã ký ĐNVV/GĐNVV qua Cus360.
4. HDB App nhận kết quả và chọn luồng `001/341` hoặc `007/741`.
5. App thực hiện hành trình vay cầm cố STK:
   - CVP/chọn sản phẩm.
   - Danh sách STK.
   - Nhập số tiền vay.
   - Xác nhận thông tin khoản vay.
   - Popup điều khoản điều kiện.
   - Popup hợp đồng tín dụng / hợp đồng vay cầm cố STK.
   - Ký số/OTP.
   - Kết quả đăng ký vay thành công.

## Rule nghiệp vụ chính

### Điều kiện trước khi vào danh sách STK

- Người nước ngoài.
- Đối tượng hạn chế cấp tín dụng.
- Trạng thái KYC.
- Giấy tờ tùy thân.
- Số điện thoại.
- Độ tuổi.

Nếu thỏa: chuyển danh sách STK. Nếu không thỏa: hiển thị thông báo tương ứng và giữ KH ở màn hình CVP.

### Điều kiện load STK

- Acctype: `ON0`, `ONL`.
- Loại tiền: VND.
- Sổ không đồng sở hữu.
- `ownership_type = SG`.
- Sổ không bị phong tỏa.
- Sổ không đang bảo đảm vay.

### Enable/disable STK

- App lấy `Số tiền vay tối đa` theo field `Số tiền có thể vay tối đa`.
- So sánh `Product Min` với `Số tiền vay tối đa`:
  - `Số tiền vay tối đa >= Product Min`: cho chọn STK.
  - `Số tiền vay tối đa < Product Min`: làm mờ STK.

### Số tiền vay tối đa

- Tiền gửi lĩnh lãi cuối kỳ:
  - Số tiền vay tối đa = Số dư tiền gửi + Lãi tiền gửi dự kiến - Chi phí liên quan tiền gửi - Lãi vay dự kiến khi đến hạn - Phí liên quan khoản vay.
- Tiền gửi lĩnh lãi định kỳ/đầu kỳ:
  - Số tiền vay tối đa = Số dư tiền gửi - Chi phí liên quan tiền gửi - Lãi vay dự kiến khi đến hạn - Phí liên quan khoản vay.
- Không vượt quá số tiền gốc của STK.
- Với sổ online và KH chưa ký GĐNVV: không vượt quá hạn mức vay online khả dụng theo TT06.
- Công thức tổng quát trong BRD:
  - `MIN[(Giá trị STK)/(1 + Lãi suất vay/365 * Thời gian), Mệnh giá, Room khả dụng, Số tiền vay tối đa theo sản phẩm, HMTT06 nếu có]`
  - Làm tròn xuống đến trăm ngàn.

### Nhập số tiền vay

- Số tiền cần vay mặc định bằng số tiền có thể vay tối đa.
- KH được điều chỉnh số tiền.
- Hiển thị min/max.
- Nếu nhập ngoài khoảng: disable button `Tiếp tục`, không cho đi tiếp.
- Ghi chú nâng hạn mức 1 tỷ chỉ hiển thị khi KH vay bằng STK online và chưa ký GĐNVV tại quầy.

### Tài khoản giải ngân

- Mặc định là tài khoản nhận lãi của STK theo rule hiện tại.
- Nếu KH không có TKTT thỏa điều kiện: để trống và cho chọn TKTT khác.
- Dùng chung danh sách TKTT với Product Code `002` - sổ tại quầy.

### Xác nhận thông tin

- Tài khoản nhận giải ngân và trích nợ tự động: không cho điều chỉnh.
- Đơn vị quản lý:
  - Sổ tại quầy: không cho điều chỉnh.
  - Sổ online: cho chọn lại qua popup.
- Email: cho điều chỉnh qua popup, validate như hiện tại.
- Điều khoản điều kiện và hợp đồng vay: hiển thị bằng popup.

### OTP / ký số

- BRD web dùng màn hình xác thực ký số FPT / màn hình nhập OTP.
- Mobile React Native hiện tại dùng popup/bottom sheet OTP.
- KH có thể bấm lấy lại OTP; app xóa OTP cũ, gửi mã mới, hiển thị message thành công/thất bại.

## Khác biệt cần chú ý so với digest PDF cũ

- MB02 ghi màn hình không có STK có button `Đăng ký nhu cầu vay`, điều hướng KH về màn hình đăng ký vay.
- Digest PDF cũ từng ghi button `Quay lại` về CVP. Khi làm task UI, cần đối chiếu design/source mới nhất để tránh dùng sai CTA.
- MB02 phần nhập tiền vay nhấn mạnh disable button nếu nhập ngoài khoảng; không mô tả rõ alert khi bấm tiếp tục.

## Out-of-scope hiện tại

- Tất toán khoản vay có trong BRD nhưng hiện người dùng chưa làm chính.
- Không lấy phần tất toán làm scope mặc định nếu người dùng chỉ nói về vay cầm cố STK hiện tại.
