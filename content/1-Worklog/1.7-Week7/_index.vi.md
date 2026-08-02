---
title: "Worklog Tuần 7"
date: 2026-07-27
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---
### Mục tiêu Tuần 7

* Tìm hiểu các nguyên lý Hạ tầng dưới dạng mã (Infrastructure as Code - IaC), cú pháp ngôn ngữ HCL (HashiCorp Configuration Language) và quy trình khai báo Terraform (`init`, `plan`, `apply`, `destroy`).
* Nắm vững quản lý Terraform State, local vs remote backend, phụ thuộc tài nguyên (`depends_on`), và biến đầu vào/đầu ra (variables/outputs).
* Mã hóa toàn bộ hạ tầng bảo mật AWS Cloud thành các module Terraform trong thư mục `terraform/`.
* Xác minh khả năng khởi tạo môi trường tự động hóa chỉ bằng một câu lệnh (`terraform apply`) cho toàn bộ thành phần Cloud.

### Các công việc triển khai trong tuần

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Nghiên cứu các khái niệm cơ bản về Terraform, cấu hình AWS Provider và nguyên tắc tổ chức mã nguồn HCL.<br>- Khởi tạo thư mục dự án Terraform (`terraform/`), cấu hình các tệp `main.tf`, `variables.tf` và `outputs.tf`. | 27/07/2026 | 27/07/2026 | <https://000102.awsstudygroup.com> |
| 3 | - Mã hóa Tầng Lưu trữ & Ghi log bằng Terraform:<br>&emsp;+ Module Amazon S3 (`s3.tf`) với mã hóa server-side, Block Public Access và chính sách bucket.<br>&emsp;+ Module AWS CloudTrail (`cloudtrail.tf`) cấu hình ghi log đa region.<br>&emsp;+ Module Amazon SQS (`sqs.tf`) kèm hàng đợi tin nhắn lỗi (DLQ). | 28/07/2026 | 28/07/2026 | <https://000037.awsstudygroup.com> |
| 4 | - Mã hóa Tầng Serverless & Tự động hóa bằng Terraform:<br>&emsp;+ Amazon EventBridge rule (`eventbridge.tf`) lọc các mẫu sự kiện an ninh CloudTrail.<br>&emsp;+ Module hàm AWS Lambda (`lambda.tf`) bao gồm đóng gói mã Python và IAM execution role.<br>&emsp;+ Module Amazon SNS topic (`sns.tf`) và đăng ký endpoint email/webhook. | 29/07/2026 | 29/07/2026 | <https://000038.awsstudygroup.com> |
| 5 | - Mã hóa Tầng NoSQL & Phân tích bằng Terraform:<br>&emsp;+ Bảng Amazon DynamoDB (`dynamodb.tf`) với key `AlertID` và cấu hình TTL.<br>&emsp;+ Module Amazon Athena database & workgroup (`athena.tf`).<br>&emsp;+ Module AWS IAM Access Analyzer (`access_analyzer.tf`). | 30/07/2026 | 30/07/2026 | <https://000102.awsstudygroup.com> |
| 6 | - Kiểm thử toàn bộ vòng đời triển khai: Chạy `terraform plan` để kiểm tra hơn 20 tài nguyên trong kế hoạch.<br>- Thực thi `terraform apply` khởi tạo toàn bộ hạ tầng đám mây trên region AWS sạch chỉ trong khoảng 3 phút.<br>- Xác minh trạng thái khởi tạo thành công và chạy thử nghiệm `terraform destroy`. | 31/07/2026 | 31/07/2026 | Terraform IaC Validation |

### Kết quả đạt được Tuần 7

* Thành thạo kỹ thuật Hạ tầng dưới dạng mã (IaC) sử dụng Terraform và ngôn ngữ HCL.
* Mã hóa hoàn toàn toàn bộ hạ tầng bảo mật AWS Cloud thành các đoạn mã Terraform dạng module trong thư mục `terraform/`.
* Thay thế hoàn toàn thao tác cấu hình thủ công trên AWS Console bằng kịch bản IaC tái sử dụng 100%.
* Chứng minh khả năng tự động hóa triển khai môi trường Cloud chỉ bằng một câu lệnh (`terraform apply`) trong chưa đầy 3 phút.
* Đảm bảo quản lý an toàn tệp Terraform state, ẩn các tham số nhạy cảm và cách ly khỏi git qua `.gitignore`.
