---
title: "Worklog Tuần 7"
date: 2026-07-27
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---
### Mục tiêu Tuần 7

* Tìm hiểu các nguyên lý Hạ tầng dưới dạng mã (Infrastructure as Code - IaC) và cú pháp ngôn ngữ HCL (HashiCorp Configuration Language).
* Mã hóa toàn bộ hạ tầng bảo mật AWS Cloud thành các module Terraform (`terraform/`: S3, CloudTrail, SQS, EventBridge, Lambda, SNS, DynamoDB, Athena, Access Analyzer).
* Xác minh khả năng khởi tạo môi trường tự động hóa chỉ với một lệnh (`terraform apply`) và dọn dẹp tài nguyên (`terraform destroy`) trong chưa đầy 3 phút.
* Thực hiện kiểm thử tích hợp toàn bộ 12 kịch bản tấn công an ninh mạng và kiểm toán tài chính AWS xác nhận chi phí đạt $0.
* Hoàn thiện toàn bộ tài liệu kỹ thuật, sơ đồ kiến trúc (`architecture.png`, `cloud-architecture.png`), và chính thức nộp toàn bộ sản phẩm dự án trước hạn chót **31/07/2026**.

### Các công việc triển khai trong tuần

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Nghiên cứu các khái niệm cơ bản về Terraform, cấu hình AWS Provider và nguyên tắc tổ chức HCL.<br>- Khởi tạo thư mục dự án Terraform (`terraform/`), cấu hình các tệp `main.tf`, `variables.tf` và `outputs.tf`. | 27/07/2026 | 27/07/2026 | <https://000102.awsstudygroup.com> |
| 3 | - Mã hóa Tầng Lưu trữ, Ghi log & Serverless bằng Terraform:<br>&emsp;+ Module Amazon S3 (`s3.tf`), CloudTrail (`cloudtrail.tf`), và Amazon SQS (`sqs.tf`).<br>&emsp;+ Module EventBridge (`eventbridge.tf`), AWS Lambda (`lambda.tf`), và Amazon SNS (`sns.tf`). | 28/07/2026 | 28/07/2026 | <https://000037.awsstudygroup.com><br><https://000038.awsstudygroup.com> |
| 4 | - Mã hóa Tầng NoSQL, Phân tích & Quét an ninh bằng Terraform:<br>&emsp;+ Module Amazon DynamoDB (`dynamodb.tf`), Amazon Athena (`athena.tf`), và IAM Access Analyzer (`access_analyzer.tf`). | 29/07/2026 | 29/07/2026 | <https://000102.awsstudygroup.com> |
| 5 | - Thực thi `terraform apply` xác minh khởi tạo tài nguyên tự động trên môi trường sạch.<br>- Tiến hành kiểm thử tích hợp toàn hệ thống trên 12 kịch bản tấn công (7 Endpoint + 5 AWS Cloud) và kiểm toán tài chính AWS ($0 spend). | 30/07/2026 | 30/07/2026 | Terraform & System Test Suite |
| 6 | - Hoàn thiện tài liệu kỹ thuật, sơ đồ kiến trúc (`architecture.png`, `cloud-architecture.png`), và đóng gói trang web Hugo (`hugo`).<br>- **Nộp dự án chính thức**: Hoàn thành và nộp toàn bộ sản phẩm cùng mã nguồn dự án trước hạn chót **31/07/2026**. | 31/07/2026 | 31/07/2026 | Final Project Submission |

### Kết quả đạt được Tuần 7

* Thành thạo kỹ thuật Hạ tầng dưới dạng mã (IaC) sử dụng Terraform và ngôn ngữ HCL.
* Mã hóa hoàn toàn toàn bộ hạ tầng bảo mật AWS Cloud thành các đoạn mã Terraform dạng module trong thư mục `terraform/`.
* Chứng minh khả năng tự động hóa triển khai môi trường Cloud chỉ bằng một câu lệnh (`terraform apply`) trong chưa đầy 3 phút.
* Xác minh tính sẵn sàng của mô hình cảnh báo kép trên 12 kịch bản tấn công và duy trì kỷ luật tài chính $0 tuyệt đối.
* Xuất bản và chính thức nộp toàn bộ sản phẩm dự án cùng cổng tài liệu SOC vào ngày **31/07/2026**.
