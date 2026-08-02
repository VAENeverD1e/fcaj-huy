---
title: "Worklog Tuần 8"
date: 2024-01-01
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---
### Mục tiêu Tuần 8

* Thiết kế và phát triển dịch vụ Backend REST API (`backend/`) bằng Python (Flask/FastAPI) kết hợp AWS Boto3 SDK cho DynamoDB và CloudWatch.
* Triển khai giao diện điều khiển Frontend (`frontend/`) bằng React, Vite và thiết kế giao diện tối (Dark Mode) hiện đại.
* Xây dựng các widget vận hành: Card trạng thái luồng dữ liệu thời gian thực, bảng giám sát chi phí Free Tier, biểu đồ độ trễ serverless và bảng dữ liệu cảnh báo an ninh trực tiếp.
* Tích hợp các component React Frontend với REST API Backend qua cơ chế tự động lấy dữ liệu theo định kỳ (30 giây/lần).

### Các công việc triển khai trong tuần

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Thiết kế kiến trúc RESTful API cho Bảng điều khiển vận hành Automation Ops Dashboard.<br>- Khởi tạo thư mục dự án Backend Python (`backend/`) và xây dựng các module dịch vụ Boto3 (`dynamodb_service.py`, `cloudwatch_service.py`, `cost_service.py`). | 03/08/2026 | 03/08/2026 | <https://000066.awsstudygroup.com> |
| 3 | - Triển khai các API Endpoint (`GET /api/alerts`, `GET /api/metrics/pipeline`, `GET /api/metrics/cost`, `GET /api/health`).<br>- Cấu hình middleware CORS, trình xử lý lỗi và cơ chế cache phản hồi API tốc độ cao (<100ms). | 04/08/2026 | 04/08/2026 | Python API Testing |
| 4 | - Khởi tạo dự án ứng dụng React sử dụng Vite (`npx create-vite@latest frontend --template react`).<br>- Thiết lập hệ thống CSS design system, typography, màu sắc Dark Mode và bố cục linh hoạt Flex/Grid. | 05/08/2026 | 05/08/2026 | <https://000079.awsstudygroup.com> |
| 5 | - Phát triển các UI component chính: `PipelineStatusWidget`, `CostTrackerWidget`, `LatencyChart` (biểu đồ đường), `InvocationChart` (biểu đồ cột), và `AlertFeedTable` (bảng dữ liệu hiển thị badge mức độ nghiêm trọng). | 06/08/2026 | 06/08/2026 | React Component Architecture |
| 6 | - Tích hợp React Frontend với các REST API Endpoint Backend (`http://localhost:5000/api`).<br>- Thêm cơ chế tự động làm mới dữ liệu sau mỗi 30 giây và kiểm thử đóng gói ứng dụng web (`npm run build`). | 07/08/2026 | 07/08/2026 | Web App Testing & Build |

### Kết quả đạt được Tuần 8

* Thiết kế và phát triển dịch vụ Backend REST API Python hiệu năng cao trong thư mục `backend/`.
* Tích hợp thư viện AWS Boto3 SDK giúp truy vấn trực tiếp các chỉ số thời gian thực từ DynamoDB, CloudWatch và AWS Billing API.
* Xây dựng giao diện web ứng dụng hiện đại chuẩn Dark Theme bằng React và Vite trong thư mục `frontend/`.
* Kết nối mượt mà React Frontend với Python Backend API kèm biểu đồ độ trễ trực quan và cơ chế làm mới telemetry mỗi 30 giây.
* Kiểm chứng thiết kế đáp ứng (responsive layout) và thời gian phản hồi API sub-100ms trên cả giao diện máy tính và di động.
