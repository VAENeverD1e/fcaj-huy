---
title: "Worklog Tuần 7"
date: 2024-01-01
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---
### Mục tiêu Tuần 7

* Tìm hiểu cơ chế quét cấu hình sai thụ động của AWS IAM Access Analyzer: Khởi tạo analyzer, đánh giá ranh giới trust zone, phát hiện quyền truy cập bên ngoài và xác minh chính sách.
* Tìm hiểu công cụ truy vấn SQL serverless Amazon Athena và vai trò trong việc truy vấn log audit CloudTrail lưu trên S3.
* Bật IAM Access Analyzer để phát hiện các quyền truy cập ngoài ý muốn vào tài nguyên AWS (S3, IAM, Lambda, SQS) mà không phát sinh chi phí duy trì.
* Xây dựng bộ 4 câu lệnh truy vấn SQL SOC nòng cốt trong Amazon Athena Console để săn đe dọa trên tệp log CloudTrail S3.

### Các công việc triển khai trong tuần

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Nghiên cứu tính năng quét thụ động của IAM Access Analyzer.<br>- Bật IAM Access Analyzer với ranh giới trust zone là tài khoản AWS.<br>- Xem xét các cảnh báo được tạo ra cho các quyền truy cập liên tài khoản (cross-account) và chia sẻ công khai S3. | 27/07/2026 | 27/07/2026 | <https://000030.awsstudygroup.com> |
| 3 | - Tìm hiểu các khái niệm truy vấn SQL serverless trên Amazon Athena.<br>- Cấu hình đường dẫn lưu kết quả truy vấn Athena trên S3 bucket. | 28/07/2026 | 28/07/2026 | <https://000106.awsstudygroup.com> |
| 4 | - Tìm hiểu cách tích hợp AWS CloudTrail với Amazon Athena.<br>- Khởi tạo bảng Athena cho log CloudTrail trên S3 bằng mẫu chuẩn của AWS. | 29/07/2026 | 29/07/2026 | <https://000040.awsstudygroup.com> |
| 5 | - Thực hành thử nghiệm 4 câu lệnh truy vấn SQL SOC trong Athena Query Editor:<br>&emsp;+ Truy vấn 1: Top các địa chỉ IP gọi API thất bại (`errorCode IS NOT NULL`).<br>&emsp;+ Truy vấn 2: Lịch sử tạo IAM access key (`CreateAccessKey`).<br>&emsp;+ Truy vấn 3: Thay đổi chính sách S3 bucket (`PutBucketPolicy`/`DeleteBucketPolicy`).<br>&emsp;+ Truy vấn 4: Các hành động API của tài khoản Root trong 7 ngày qua. | 30/07/2026 | 30/07/2026 | <https://000106.awsstudygroup.com> |
| 6 | - Kiểm tra kết quả thực thi và thời gian truy vấn trên Athena Console.<br>- Xác nhận dịch vụ Athena và IAM Access Analyzer không phát sinh chi phí hạ tầng duy trì hàng tháng. | 31/07/2026 | 31/07/2026 | AWS Cost Management |

### Kết quả đạt được Tuần 7

* Bật thành công AWS IAM Access Analyzer giúp tự động phát hiện cấu hình sai thụ động (miễn phí vĩnh viễn).
* Cấu hình Athena Query Editor và khởi tạo bảng log CloudTrail sử dụng mẫu có sẵn trên AWS console.
* Xây dựng bộ 4 câu lệnh truy vấn SQL điều tra sự cố thực tế giúp săn đe dọa trực tiếp trên tệp log CloudTrail S3 thô.
* Xác nhận tính năng quét thụ động và truy vấn Athena theo nhu cầu không làm phát sinh chi phí hạ tầng hàng tháng.
