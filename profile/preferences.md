# Preferences

## Ưu tiên kỹ thuật

- Ưu tiên UI tốt, cảm giác giao diện, layout, style, CSS.
- Quan tâm mạnh đến cách tổ chức thông tin sao cho dễ tìm lại và đọc lại.
- Khi ghi lỗi hoặc task, không chỉ liệt kê kết quả; cần ghi rõ nguyên nhân, cách sửa, file nào sửa gì, vì sao sửa.

## Ưu tiên khi agent làm việc

- Agent nên đọc dữ liệu trong WikiLLM trước khi tư vấn hoặc xử lý task.
- Agent nên cập nhật dữ liệu hằng ngày để tăng độ hiểu người dùng.
- Agent nên tránh suy diễn quá mức nếu thiếu dữ kiện; phần chưa rõ thì ghi rõ là chưa rõ.
- Agent nên ưu tiên hành động cụ thể, ghi nhận rõ ràng, và cập nhật lại profile/memory khi người dùng cung cấp thông tin mới.

## Quyền xử lý task

- Khi cần scan project, dò lỗi, đọc file, chạy command để mò bug hoặc xác minh bug: agent có thể làm, không cần hỏi lại trước.
- Nếu thao tác có khả năng liên quan đến bảo mật, secret, token, credential, thông tin nội bộ, dữ liệu nhạy cảm, hoặc có khả năng rò rỉ dữ liệu ra bên ngoài: hỏi người dùng confirm trước.
- Nếu cần dùng network, gửi log/code/data ra dịch vụ bên ngoài, upload file, cài package từ nguồn bên ngoài, hoặc gọi API ngoài workspace: hỏi người dùng confirm trước.
- Khi người dùng yêu cầu trực tiếp cập nhật WikiLLM/profile/report, agent được cập nhật các file trong WikiLLM theo yêu cầu đó.
- Nếu quyền thao tác không rõ thuộc nhóm nào, ưu tiên hỏi ngắn gọn trước khi làm.

## Điều nên tránh

- Tránh giải thích dài dòng.
- Tránh nói vòng vo, lịch sự quá mức, hoặc cả nể.
- Tránh làm cho bản ghi task/lỗi thành danh sách rời rạc khó đọc lại.
