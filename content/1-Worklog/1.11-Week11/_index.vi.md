---
title: "Worklog Tuần 11"
date: 2024-01-01
weight: 11
chapter: false
pre: " <b> 1.11. </b> "
---
### Mục tiêu Tuần 11

* Học cách thiết kế component, quản lý trạng thái (`useState`, `useEffect`), gọi REST API (`fetch` / `axios`) và tích hợp thư viện biểu đồ trực quan hóa dữ liệu.
* Xây dựng giao diện frontend web (`frontend/`) cho **Bảng điều khiển vận hành tự động (Automation Ops Dashboard)**.
* Xây dựng các widget điều khiển tương tác hiển thị sức khỏe đường ống tự động hóa, số lượt gọi dịch vụ, biểu đồ độ trễ xử lý, bảng cảnh báo an ninh thời gian thực và chỉ số chi phí Free Tier.

### Các công việc triển khai trong tuần

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Khởi tạo dự án ứng dụng React sử dụng Vite (`npx create-vite@latest frontend --template react`).<br>- Thiết kế hệ thống giao diện CSS hiện đại, typography, bảng màu dark mode và bố cục responsive flex/grid. | 24/08/2026 | 24/08/2026 | <https://000079.awsstudygroup.com> |
| 3 | - Phát triển các Component Status Card nòng cốt:<br>&emsp;+ `PipelineStatusWidget`: Hiển thị trạng thái sức khỏe hệ thống (Healthy / Degraded / Down).<br>&emsp;+ `CostTrackerWidget`: Hiển thị chi phí AWS hiện tại so với hạn mức ngân sách $0 Free Tier. | 25/08/2026 | 25/08/2026 | React Component Architecture |
| 4 | - Phát triển các Component Biểu đồ Chỉ số:<br>&emsp;+ `LatencyChart`: Biểu đồ đường thể hiện thời gian thực thi Lambda & độ trễ xử lý hàng đợi SQS.<br>&emsp;+ `InvocationChart`: Biểu đồ cột thể hiện lưu lượng sự kiện trong 24 giờ. | 26/08/2026 | 26/08/2026 | Charting Library Integration |
| 5 | - Phát triển Component Bảng Cảnh báo An ninh Thời gian thực:<br>&emsp;+ `AlertFeedTable`: Bảng dữ liệu tương tác hiển thị danh sách cảnh báo lấy từ API `/api/alerts`.<br>&emsp;+ Tính năng: badge màu phân cấp mức độ nghiêm trọng, tìm kiếm từ khóa, pop-up xem chi tiết CloudTrail JSON audit. | 27/08/2026 | 27/08/2026 | Data Table & Modal UI |
| 6 | - Tích hợp React frontend với các API backend Flask/FastAPI (`http://localhost:5000/api`).<br>- Thêm cơ chế tự động làm mới dữ liệu sau mỗi 30 giây.<br>- Kiểm chứng đóng gói ứng dụng web thành công (`npm run build`). | 28/08/2026 | 28/08/2026 | Web App Testing & Build |

### Kết quả đạt được Tuần 11

* Xây dựng thành công giao diện web dashboard dark-mode hiện đại bằng React trong thư mục `frontend/`.
* Triển khai thành công các biểu đồ trực quan hóa dữ liệu động theo dõi lưu lượng và độ trễ xử lý hệ thống serverless.
* Tích hợp bảng danh sách cảnh báo thời gian thực hiển thị dữ liệu sự kiện an ninh lấy trực tiếp từ Amazon DynamoDB.
* Kết nối mượt mà React frontend với API backend Python, hỗ trợ cơ chế tự động cập nhật dữ liệu telemetry sau mỗi 30 giây.
* Kiểm chứng giao diện hiển thị tối ưu và phản hồi mượt mà trên cả màn hình máy tính và thiết bị di động.
