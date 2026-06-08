# Open Questions

File này lưu các điểm còn thiếu/chưa rõ để agent biết không được tự đoán quá xa khi nhận task.

## Cách dùng

- Khi gặp thông tin còn thiếu nhưng chưa cần hỏi ngay, ghi vào đây.
- Khi người dùng bổ sung dữ liệu, cập nhật hoặc xóa câu hỏi tương ứng.
- Không lưu task ở đây. Task chỉ lưu trong `tasks/` khi người dùng nói rõ đó là task.

## hdb_mfe_loan - Vay cầm cố sổ tiết kiệm

### API / integration

- Đã có technical spec MB03 và Postman index cho nhiều API Digital Lending.
- Người dùng đã confirm source webapp `/Users/atien/work/web/lending_uat_2026` dùng endpoint group `/api-dl/*`.
- Cần xác nhận source React Native `/Users/atien/work/app/hdb_mfe_loan` map sang endpoint nào và khác webapp cũ ra sao.
- Cần xác nhận API/source hiện tại đang map Product Code `007`, Product Code `001`, LoanSubType `741`, LoanSubType `341` ở đâu.
- Cần xác nhận field BE thực tế có đúng theo spec:
  - `depositAcctAccruedAmt`
  - `availableOutstanding`
  - `minLoanAmount`
  - `isSigned`
  - `loanSubType`
- Cần rule kỹ thuật hiện tại của FPT eSign/OTP:
  - Thời gian hết hạn OTP.
  - Số lần nhập sai.
  - Số lần resend tối đa.
- Đã có file message lỗi: `documents/errorMessage.json`.
- Đã có message `REQUEST_OTP_IS_OVER_LIMIT_PER_5_MIN`, nhưng file chưa nói rõ số lần resend tối đa.
- Cần xác nhận sau task pentest, cơ chế AES256 encrypt body đã đổi thế nào:
  - Secret đã được remove khỏi FE chưa.
  - FE còn encrypt body không.
  - Nếu còn encrypt, key/session material lấy từ đâu.

### Design / flow

- Cần prototype/design riêng cho bản React Native TypeScript nếu có.
- BRD/web ReactJS có màn hình nhập OTP riêng, nhưng bản mobile React Native chỉ có popup/bottom sheet OTP. Khi làm RN task, vẫn cần ưu tiên design/source mobile.
- Cần tài liệu integration với `hdb_host`: route/event mở miniapp, params truyền vào, rule back/home, rule đóng miniapp.
- Cần đối chiếu màn hình không có STK:
  - Digest PDF cũ ghi CTA `Quay lại` về CVP.
  - MB02 docx ghi CTA `Đăng ký nhu cầu vay` điều hướng về màn hình đăng ký vay.
  - Khi làm UI task, cần kiểm tra design/source mới nhất.

### Scope

- Tất toán khoản vay có trong BRD nhưng hiện người dùng chưa làm. Không lấy phần tất toán làm scope chính nếu người dùng chưa yêu cầu.
- Cần URD hoặc note từ BA/PM cho bản mobile mới nếu khác BRD web cũ.

## hdb_mfe_od - Vay thấu chi

- Wiki path: `projects/hdb_mfe_od`.
- Source path: `/Users/atien/work/app/hdb_mfe_od`.
- Người dùng không làm chính project này.
- Chưa thu thập đủ tài liệu BRD/URD/technical docs. Người dùng sẽ bổ sung sau.
