# Tech Stack

## React Native

- Version thường gặp: `0.77.0`.
- Navigation: `react-navigation`.
- State management:
  - Redux Toolkit
  - React Context
- Styling:
  - Ưu tiên atomic CSS style.
  - Có dùng/quan tâm Tailwind CSS.

## React Web

- Có làm React bằng JavaScript và TypeScript.
- Routing:
  - `react-router-dom`
  - `react-router`
  - Version tùy dự án, thường gặp v5 hoặc v6.

## UI / Styling

- Người dùng có nền tảng và sở thích mạnh về UI/CSS.
- Khi tư vấn frontend/mobile UI, nên ưu tiên cách nghĩ theo component, layout, spacing, style system, reusable patterns.
- Style preference: gọn, atomic, dễ đọc, dễ chỉnh.

## Cách agent nên dùng thông tin stack

- Khi gặp task mobile, mặc định ưu tiên bối cảnh React Native `0.77.0` nếu chưa có thông tin khác.
- Khi gặp task web React, kiểm tra version router trước khi đề xuất API vì `react-router` v5 và v6 khác nhau đáng kể.
- Khi đề xuất styling, ưu tiên atomic CSS/Tailwind-like approach nếu phù hợp với project.
