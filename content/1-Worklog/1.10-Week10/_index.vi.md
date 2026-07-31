---
title: "Worklog Tuần 10"
date: 2024-01-01
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---
### Mục tiêu Tuần 10

* Phân tích các khái niệm lập trình web backend bằng Python: Thiết kế chuẩn RESTful API, HTTP status code, cấu hình CORS middleware và biến môi trường.
* Học cách tích hợp thư viện AWS Boto3 SDK để truy vấn dữ liệu từ Amazon DynamoDB và các chỉ số vận hành Amazon CloudWatch.
* Xây dựng dịch vụ backend (`backend/`) cho **Bảng điều khiển vận hành tự động (Automation Ops Dashboard)** bằng Python (Flask / FastAPI).
* Xây dựng các API endpoint trả về trạng thái thời gian thực của đường ống tự động hóa, lịch sử cảnh báo, số lượt gọi Lambda, độ trễ xử lý và chi phí AWS Free Tier.

### Các công việc triển khai trong tuần

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Thiết kế kiến trúc RESTful API cho Automation Ops Dashboard.<br>- Khởi tạo thư mục dự án Python (`backend/`), cấu hình môi trường ảo (`venv`), và cài đặt các thư viện phụ thuộc (`Flask`/`FastAPI`, `boto3`, `flask-cors`, `python-dotenv`). | 17/08/2026 | 17/08/2026 | <https://000066.awsstudygroup.com> |
| 3 | - Xây dựng module tích hợp Boto3 DynamoDB (`backend/services/dynamodb_service.py`).<br>- Triển khai API endpoint `GET /api/alerts`: truy vấn bảng `SecurityAlerts` có phân trang, lọc theo mức độ nghiêm trọng và khoảng thời gian. | 18/08/2026 | 18/08/2026 | <https://000060.awsstudygroup.com> |
| 4 | - Xây dựng module tích hợp Boto3 CloudWatch (`backend/services/cloudwatch_service.py`).<br>- Triển khai API endpoint `GET /api/metrics/pipeline`: lấy dữ liệu số lượt gọi Lambda, thời gian thực thi, độ sâu hàng đợi SQS và tỷ lệ lỗi. | 19/08/2026 | 19/08/2026 | <https://000008.awsstudygroup.com> |
| 5 | - Xây dựng module theo dõi chi phí Free Tier (`backend/services/cost_service.py`).<br>- Triển khai API endpoint `GET /api/metrics/cost`: truy vấn dữ liệu AWS Budgets / CloudWatch estimated charges để báo cáo chi phí hiện tại so với hạn mức $0. | 20/08/2026 | 20/08/2026 | <https://000007.awsstudygroup.com> |
| 6 | - Cấu hình CORS middleware, xử lý ngoại lệ lỗi API và caching câu trả lời.<br>- Viết unit test (`pytest`) cho các API endpoint.<br>- Kiểm chứng hoạt động của dịch vụ backend tại local (`http://localhost:5000/api/health`). | 21/08/2026 | 21/08/2026 | Python API Testing |

### Kết quả đạt được Tuần 10

* Thiết kế và triển khai thành công dịch vụ backend Python REST API hiệu năng cao trong thư mục `backend/`.
* Tích hợp thành công AWS Boto3 SDK để truy vấn dữ liệu thời gian thực từ DynamoDB, CloudWatch và AWS Billing API.
* Xây dựng 4 endpoint vận hành nòng cốt: `/api/alerts`, `/api/metrics/pipeline`, `/api/metrics/cost`, và `/api/health`.
* Xây dựng cơ chế xử lý lỗi chặt chẽ và hỗ trợ CORS phục vụ kết nối mượt mà với ứng dụng frontend web.
* Xác minh độ trễ phản hồi của API backend đạt mức dưới 100ms cho các tác vụ tổng hợp dữ liệu telemetry.
