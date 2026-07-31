---
title: "Worklog Tuần 6"
date: 2024-01-01
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---
### Mục tiêu Tuần 6

* Hiểu các khái niệm cơ sở dữ liệu NoSQL Amazon DynamoDB: Partition key, sort key, secondary index, chế độ dung lượng (Pay-Per-Request / Provisioned Free Tier) và Time To Live (TTL).
* Nâng cấp hàm Lambda để lưu trữ toàn bộ bản ghi cảnh báo an ninh vào DynamoDB phục vụ tra cứu lịch sử audit và hiển thị trên giao diện điều khiển.
* Nắm vững dịch vụ giám sát Amazon CloudWatch: Custom Metric, Metric Filter, CloudWatch Alarm và cơ chế kích hoạt thông báo.
* Thiết lập hệ thống giám sát vận hành và rào chắn chi phí cho đường ống tự động hóa serverless.

### Các công việc triển khai trong tuần

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Nghiên cứu chuyên sâu kiến trúc NoSQL Amazon DynamoDB.<br>- Tạo bảng DynamoDB (`SecurityAlerts`) với `AlertID` (Partition Key) và `Timestamp` (Sort Key).<br>- Bật thuộc tính Time To Live (TTL) (`ttl_expiry`) thời hạn 90 ngày để tự động giải phóng dung lượng lưu trữ, đảm bảo hạn mức Free Tier. | 20/07/2026 | 20/07/2026 | <https://000060.awsstudygroup.com> |
| 3 | - Cập nhật hàm Lambda (`soc-alert-enricher`) sử dụng Boto3 `dynamodb.put_item()` ghi lại mọi cảnh báo đã xử lý vào bảng `SecurityAlerts`.<br>- Lưu kèm dữ liệu phong phú: mức độ nghiêm trọng (severity), IP tấn công, AWS account ID, raw event JSON và hướng dẫn xử lý. | 21/07/2026 | 21/07/2026 | <https://000060.awsstudygroup.com> |
| 4 | - Tìm hiểu Amazon CloudWatch Metrics, Alarms và Metric Filters.<br>- Tạo CloudWatch Metric Filter trên luồng log SQS queue để theo dõi tốc độ xử lý tin nhắn và số lượng tin nhắn lỗi trong DLQ. | 22/07/2026 | 22/07/2026 | <https://000008.awsstudygroup.com> |
| 5 | - Cấu hình các CloudWatch Alarm:<br>&emsp;+ `SQSQueueDepthAlarm`: Kích hoạt nếu độ sâu hàng đợi SQS vượt 500 tin nhắn (phát hiện nghẽn dữ liệu về SIEM).<br>&emsp;+ `LambdaErrorAlarm`: Kích hoạt khi có lỗi thực thi hàm Lambda.<br>&emsp;+ `FreeTierBudgetAlarm`: Theo dõi chi phí dự báo hàng tháng. | 23/07/2026 | 23/07/2026 | <https://000036.awsstudygroup.com> |
| 6 | - Kiểm thử kiến trúc cảnh báo song song: Xác nhận khi mô phỏng tấn công, hệ thống đồng thời gửi cảnh báo SNS tức thì, ghi bản ghi vào DynamoDB và đẩy dữ liệu về Elastic SIEM.<br>- Kiểm tra các chỉ số hiển thị trên CloudWatch console. | 24/07/2026 | 24/07/2026 | End-to-End Pipeline Testing |

### Kết quả đạt được Tuần 6

* Làm quen thực hành thiết kế cơ sở dữ liệu NoSQL với Amazon DynamoDB và tự động hóa quản lý vòng đời dữ liệu (TTL).
* Xây dựng kho lưu trữ cảnh báo an ninh bền vững ghi nhận toàn bộ telemetry đe dọa vào DynamoDB.
* Cấu hình hệ thống cảnh báo CloudWatch Alarms đảm bảo giám sát sức khỏe vận hành của hàm Lambda và độ sâu hàng đợi SQS.
* Kiểm chứng thành công kiến trúc cảnh báo song song: luồng serverless cảnh báo gần như tức thì (~2 giây) chạy song song với luồng phân tích SIEM (~5 phút).
* Đảm bảo toàn bộ tài nguyên hoạt động hoàn toàn trong hạn mức AWS Free Tier (25 GB lưu trữ DynamoDB miễn phí, 10 CloudWatch alarms miễn phí).
