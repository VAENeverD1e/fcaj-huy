---
title: "Worklog Tuần 5"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---
### Mục tiêu Tuần 5

* Hiểu các nguyên lý kiến trúc hướng sự kiện trên AWS (Event-Driven Architecture): Event producer, event bus, event rule và target.
* Nắm vững cách tạo luật Amazon EventBridge bằng cú pháp JSON event pattern lọc sự kiện an ninh CloudTrail.
* Tìm hiểu mô hình thực thi serverless AWS Lambda, IAM execution role, biến môi trường và tích hợp thư viện Python Boto3 SDK.
* Tìm hiểu dịch vụ nhắn tin Amazon SNS: Tạo topic, cấu hình chính sách truy cập, đăng ký nhận cảnh báo qua email/HTTP webhook và định dạng tin nhắn.
* Thiết kế đường ống phát cảnh báo thời gian thực dạng serverless: EventBridge → Lambda → SNS.

### Các công việc triển khai trong tuần

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Nghiên cứu kiến trúc AWS EventBridge, custom event bus và cú pháp JSON event pattern.<br>- Tạo Amazon SNS Topic (`soc-security-alerts-topic`) và đăng ký endpoint nhận tin qua email/webhook. | 13/07/2026 | 13/07/2026 | <https://000077.awsstudygroup.com> |
| 3 | - Viết các EventBridge Event Rule lọc các sự kiện CloudTrail quan trọng:<br>&emsp;+ Đăng nhập tài khoản Root không có MFA<br>&emsp;+ Chỉnh sửa chính sách IAM (`AttachUserPolicy`, `PutUserPolicy`)<br>&emsp;+ Các nỗ lực truy cập không hợp lệ (`UnauthorizedOperation`). | 14/07/2026 | 14/07/2026 | <https://000054.awsstudygroup.com> |
| 4 | - Tìm hiểu cách triển khai AWS Lambda, môi trường Python runtime và IAM execution role (`LambdaSOCAlertExecutionRole`).<br>- Viết hàm Lambda (`soc-alert-enricher`) sử dụng Boto3 để đọc dữ liệu event payload từ EventBridge. | 15/07/2026 | 15/07/2026 | <https://000022.awsstudygroup.com> |
| 5 | - Xây dựng logic làm giàu dữ liệu trong Lambda: trích xuất IP gọi API, AWS account ID, loại user, thời gian và hành động API.<br>- Cấu hình Lambda định dạng tin nhắn dễ đọc và xuất trực tiếp tới SNS Topic qua Boto3. | 16/07/2026 | 16/07/2026 | <https://000022.awsstudygroup.com> |
| 6 | - Gán EventBridge rule chuyển dữ liệu tới target là hàm Lambda.<br>- Chạy thử nghiệm các kịch bản tấn công Cloud để kiểm tra việc nhận cảnh báo thời gian thực.<br>- Đo lường độ trễ đường ống: đạt mức phát cảnh báo gần như tức thì (~2–5 giây). | 17/07/2026 | 17/07/2026 | Serverless Pipeline Testing |

### Kết quả đạt được Tuần 5

* Thành thạo mô hình kiến trúc hướng sự kiện với Amazon EventBridge và AWS Lambda serverless.
* Cấu hình thành công dịch vụ phát tin Amazon SNS gửi cảnh báo an ninh tức thì.
* Lập trình hàm Python Lambda phân tích và bổ sung ngữ cảnh cho dữ liệu log audit CloudTrail JSON.
* Xây dựng luồng cảnh báo serverless với độ trễ dưới 5 giây, hoạt động song song với luồng phân tích chuyên sâu trên SIEM (~5 phút).
* Đảm bảo 100% tuân thủ hạn mức Free Tier (nằm trong ngưỡng 1 triệu yêu cầu Lambda và 100.000 tin nhắn SNS miễn phí mỗi tháng).
