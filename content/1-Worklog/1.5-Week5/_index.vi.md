---
title: "Worklog Tuần 5"
date: 2026-07-13
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---
### Mục tiêu Tuần 5

* Hiểu các nguyên lý kiến trúc hướng sự kiện trên AWS (Event-Driven Architecture): Event producer, event bus, event rule và target.
* Nắm vững cách tạo luật Amazon EventBridge bằng cú pháp JSON event pattern lọc sự kiện an ninh CloudTrail.
* Tìm hiểu mô hình thực thi serverless AWS Lambda, IAM execution role và tích hợp thư viện Python Boto3 SDK.
* Tìm hiểu dịch vụ nhắn tin Amazon SNS (cảnh báo email) và cơ sở dữ liệu NoSQL Amazon DynamoDB (bảng `SecurityAlerts` kèm cơ chế lưu trữ TTL).
* Xây dựng và kiểm chứng đường ống cảnh báo an ninh serverless thời gian thực: EventBridge → Lambda → SNS + kho lưu trữ DynamoDB.

### Các công việc triển khai trong tuần

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Nghiên cứu kiến trúc AWS EventBridge, custom event bus và cú pháp JSON event pattern.<br>- Tạo Amazon SNS Topic (`soc-security-alerts-topic`) và đăng ký endpoint nhận tin qua email/webhook. | 13/07/2026 | 13/07/2026 | <https://000077.awsstudygroup.com> |
| 3 | - Viết các EventBridge Event Rule lọc các sự kiện CloudTrail quan trọng (`RootNoMFA`, chỉnh sửa IAM policy, truy cập trái phép).<br>- Khởi tạo bảng Amazon DynamoDB (`SecurityAlerts`) với Partition Key `AlertID` và cấu hình TTL 90 ngày. | 14/07/2026 | 14/07/2026 | <https://000060.awsstudygroup.com> |
| 4 | - Lập trình hàm Python AWS Lambda (`soc-alert-enricher`) kèm IAM execution role.<br>- Triển khai logic Boto3 làm giàu dữ liệu sự kiện CloudTrail và gửi cảnh báo thời gian thực tới SNS Topic. | 15/07/2026 | 15/07/2026 | <https://000022.awsstudygroup.com> |
| 5 | - Nâng cấp hàm Lambda ghi bản ghi cảnh báo đã xử lý vào bảng DynamoDB (`dynamodb.put_item()`).<br>- Cấu hình các CloudWatch Alarm (`SQSQueueDepthAlarm`, `LambdaErrorAlarm`, `FreeTierBudgetAlarm`). | 16/07/2026 | 16/07/2026 | <https://000008.awsstudygroup.com> |
| 6 | - Gán EventBridge rule tới target hàm Lambda.<br>- Thực thi kịch bản tấn công Cloud kiểm thử luồng cảnh báo kép: gửi email SNS tức thì (~2s) và lưu trữ log audit lâu dài trên DynamoDB. | 17/07/2026 | 17/07/2026 | Serverless Pipeline Testing |

### Kết quả đạt được Tuần 5

* Thành thạo mô hình kiến trúc hướng sự kiện với Amazon EventBridge, AWS Lambda và Amazon SNS.
* Lập trình hàm Python Lambda phân tích, bổ sung ngữ cảnh cho log CloudTrail JSON và gửi thông báo tức thì.
* Xây dựng kho lưu trữ cảnh báo an ninh bền vững ghi nhận toàn bộ telemetry đe dọa trực tiếp vào DynamoDB.
* Cấu hình hệ thống CloudWatch Alarms đảm bảo giám sát sức khỏe vận hành của hàm Lambda và độ sâu hàng đợi SQS.
* Đảm bảo luồng cảnh báo serverless hoạt động gần như tức thì (<5s) và tuân thủ 100% hạn mức AWS Free Tier.
