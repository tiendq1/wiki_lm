# Open Questions

File này lưu các điểm còn thiếu/chưa rõ để agent biết không được tự đoán quá xa khi nhận task.

## Cách dùng

- Khi gặp thông tin còn thiếu nhưng chưa cần hỏi ngay, ghi vào đây.
- Khi người dùng bổ sung dữ liệu, cập nhật hoặc xóa câu hỏi tương ứng.
- Không lưu task ở đây. Task chỉ lưu trong `tasks/` khi người dùng nói rõ đó là task.

## hdb_mfe_loan - Vay cầm cố sổ tiết kiệm

### API / integration

- Cần API spec chi tiết cho API HDB App gọi Cus360 để kiểm tra trạng thái KH đã ký GĐNVV/ĐNVV:
  - Endpoint.
  - Request params/body.
  - Response fields.
  - Error codes.
  - Rule retry/cache nếu có.
- Cần mapping field BE trả về cho danh sách STK:
  - `Số tiền có thể vay tối đa`.
  - `Product Min`.
  - Hạn mức TT06 khả dụng.
  - Room khả dụng.
  - Lãi suất vay.
  - Ngày đến hạn khoản vay.
  - Phí/chi phí liên quan nếu FE cần hiển thị hoặc tính toán.
- Cần xác nhận API/source hiện tại đang map Product Code `007` và LoanSubType `741` ở đâu.
- Cần rule kỹ thuật hiện tại của FPT eSign/OTP:
  - Thời gian hết hạn OTP.
  - Số lần resend.
  - Số lần nhập sai.
  - Format message lỗi.

### Design / flow

- Cần prototype/design riêng cho bản React Native TypeScript nếu có.
- BRD/web ReactJS có màn hình nhập OTP riêng, nhưng bản mobile React Native chỉ có popup/bottom sheet OTP. Khi làm RN task, vẫn cần ưu tiên design/source mobile.
- Cần tài liệu integration với `hdb_host`: route/event mở miniapp, params truyền vào, rule back/home, rule đóng miniapp.

### Scope

- Tất toán khoản vay có trong BRD nhưng hiện người dùng chưa làm. Không lấy phần tất toán làm scope chính nếu người dùng chưa yêu cầu.
- Cần URD hoặc note từ BA/PM cho bản mobile mới nếu khác BRD web cũ.

## hdb_mfe_od - Vay thấu chi

- Wiki path: `projects/hdb_mfe_od`.
- Source path: `/Users/atien/work/app/hdb_mfe_od`.
- Người dùng không làm chính project này.
- Chưa thu thập đủ tài liệu BRD/URD/technical docs. Người dùng sẽ bổ sung sau.
