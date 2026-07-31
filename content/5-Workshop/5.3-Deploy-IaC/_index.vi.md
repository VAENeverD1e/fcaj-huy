---
title: "Triển khai Hạ tầng IaC"
date: 2026-07-29
weight: 3
chapter: false
pre: " <b> 5.3. </b> "
---

# 5.3 Triển khai Hạ tầng dưới dạng Mã (IaC) với Terraform & Kiểm tra trên AWS

#### Tổng quan

Trong phần này, bạn sẽ tự tay khởi tạo toàn bộ hạ tầng AWS Cloud cho SOC Detection Lab bằng **Terraform Infrastructure as Code (IaC)**, xem xét trực tiếp các đoạn mã nguồn HCL, và kiểm tra từng tài nguyên được khởi tạo thông qua cả **AWS Management Console** và **AWS CLI**.

---

#### Cấu trúc Thư mục Mã Hạ tầng

Toàn bộ cấu hình Terraform nằm trong thư mục `terraform/`:

```
terraform/
├── main.tf                 # Khai báo nhà cung cấp (provider) và thiết lập chung
├── variables.tf            # Các tham số đầu vào (region, tên bucket, nhãn môi trường)
├── outputs.tf              # Xuất các thông số ARN và endpoint sau khi triển khai
├── cloudtrail.tf           # Khởi tạo CloudTrail đa region, S3 log bucket, mã hóa KMS
├── s3.tf                   # Cấu hình chính sách S3 Bucket và đẩy thông báo sự kiện sang SQS
├── sqs.tf                  # Hàng chờ lỗi SQS DLQ & Hàng chờ SQS cho Elastic Agent kéo log
├── eventbridge.tf          # Luật EventBridge điều hướng sự kiện sang Step Functions
├── step_functions.tf       # State machine phối hợp tự động hóa (Detect -> Enrich -> Decide -> Remediate)
├── lambda.tf               # Hàm AWS Lambda xử lý cảnh báo & tự động phản ứng remediate
├── lambda_remediation.tf   # Phân quyền IAM tối thiểu cho Lambda remediate
├── athena.tf               # Athena Workgroup & DDL bảng Glue Catalog hỗ trợ SQL hunting
├── guardduty.tf            # Cấu hình GuardDuty Foundational Detector & S3 Protection
├── security_hub.tf         # Kích hoạt Security Hub & Config recorder theo chuẩn CIS Benchmark
├── dynamodb.tf             # Bảng DynamoDB lưu nhật ký audit telemetry tự động hóa
├── sns.tf                  # Topic SNS gửi cảnh báo và endpoint đăng ký
└── cloudwatch.tf           # Các cảnh báo CloudWatch Alarm theo dõi sức khỏe đường ống
```

---

#### Các Đoạn Mã Hạ tầng Quan trọng (HCL Snippets)

##### 1. Khởi tạo CloudTrail & S3 Log Bucket (`cloudtrail.tf`)
```hcl
resource "aws_cloudtrail" "soc_trail" {
  name                          = "soc-detection-lab-trail"
  s3_bucket_name                = aws_s3_bucket.cloudtrail_logs.id
  include_global_service_events = true
  is_multi_region_trail         = true
  enable_logging                = true
  enable_log_file_validation    = true

  event_selector {
    read_write_type           = "All"
    include_management_events = true
  }
}
```

##### 2. Luật EventBridge Lọc Mẫu Tấn công (`eventbridge.tf`)
```hcl
resource "aws_cloudwatch_event_rule" "api_threat_rule" {
  name        = "soc-cloudtrail-threat-rule"
  description = "Filter high-severity API threat actions for instant serverless notification"

  event_pattern = jsonencode({
    "source": ["aws.iam", "aws.s3"],
    "detail-type": ["AWS API Call via CloudTrail"],
    "detail": {
      "eventName": [
        "CreateUser",
        "AttachUserPolicy",
        "PutBucketPolicy",
        "DeleteBucketPolicy"
      ]
    }
  })
}

resource "aws_cloudwatch_event_target" "lambda_target" {
  rule      = aws_cloudwatch_event_rule.api_threat_rule.name
  target_id = "SendToLambdaHandler"
  arn       = aws_lambda_function.alert_processor.arn
}
```

##### 3. Hàm Lambda Xử lý Cảnh báo & IAM Role (`lambda.tf`)
```hcl
resource "aws_lambda_function" "alert_processor" {
  filename         = "lambda_function.zip"
  function_name    = "soc-alert-processor"
  role             = aws_iam_role.lambda_exec_role.arn
  handler          = "lambda_function.lambda_handler"
  runtime          = "python3.11"
  timeout          = 15

  environment {
    variables = {
      DISCORD_WEBHOOK_URL = var.discord_webhook_url
      DYNAMODB_TABLE_NAME = aws_dynamodb_table.pipeline_telemetry.name
    }
  }
}
```

##### 4. Hàng chờ SQS Thu thập Dữ liệu cho Elastic Fleet (`sqs.tf`)
```hcl
resource "aws_sqs_queue" "cloudtrail_sqs" {
  name                       = "soc-cloudtrail-sqs"
  delay_seconds              = 0
  max_message_size           = 262144
  message_retention_seconds  = 86400
  receive_wait_time_seconds  = 10
  visibility_timeout_seconds = 300
}
```

---

#### Phương án A: Triển khai Hạ tầng Tự động qua Terraform CLI

Thực hiện chính xác các lệnh terminal sau để tự động hóa khởi tạo toàn bộ hạ tầng:

1. **Truy cập Thư mục Terraform**:
   ```bash
   cd terraform/
   ```

2. **Cấu hình File Biến Tùy chỉnh (`terraform.tfvars`)**:
   Tạo file `terraform.tfvars`:
   ```hcl
   aws_region          = "ap-southeast-2"
   environment         = "production"
   discord_webhook_url = "https://discord.com/api/webhooks/YOUR_WEBHOOK_URL"
   ```

3. **Khởi tạo Modules & Providers**:
   ```bash
   terraform init
   ```

4. **Kiểm tra & Xem trước Kế hoạch Triển khai**:
   ```bash
   terraform plan -out=tfplan
   ```

5. **Thực thi Triển khai Hạ tầng**:
   ```bash
   terraform apply tfplan
   ```

![Terraform Deployment Execution Terminal Output](/images/5-Workshop/5.3-Deploy-IaC/terraform_apply.png)

---

#### Phương án B: Hướng dẫn Triển khai Thủ công qua AWS Console (Chi tiết Từng bước)

Dành cho người thực hành muốn tự tay cấu hình từng dịch vụ trên AWS Console để hiểu rõ logic kiến trúc:

##### Bước 1: Tạo Amazon S3 Central Log Bucket & Hàng chờ SQS Queue
1. Mở **Amazon S3 Console** → Nhấn **Create bucket**. Tên: `soc-cloudtrail-logs-[MÃ_TÀI_KHOẢN_CỦA_BẠN]`, Region: `ap-southeast-2`. Bật **mã hóa SSE-S3**.
2. Mở **Amazon SQS Console** → Nhấn **Create queue**. Loại: Standard. Tên: `soc-cloudtrail-sqs`. Cấu hình Visibility Timeout: `300` giây.

##### Bước 2: Cấu hình Luồng Ghi Log AWS CloudTrail Trail
1. Mở **AWS CloudTrail Console** → Chọn **Trails** → Nhấn **Create trail**.
2. Tên: `soc-detection-lab-trail`. Kho lưu trữ: Chọn bucket S3 vừa tạo `soc-cloudtrail-logs-[MÃ_TÀI_KHOẢN_CỦA_BẠN]`.
3. Bật **Multi-region trail** và **Management events** (`Read/Write: All`).

##### Bước 3: Tạo Bảng Kiểm toán Amazon DynamoDB Table
1. Mở **Amazon DynamoDB Console** → Nhấn **Create table**.
2. Tên bảng: `automation-pipeline-events` (bắt buộc dùng dấu gạch ngang). Partition key: `event_id` (Kiểu chuỗi `S`). Capacity: On-Demand.

##### Bước 4: Tạo Hàm AWS Lambda Auto-Remediation Function
1. Mở **AWS Lambda Console** → Nhấn **Create function**. Tên hàm: `soclab-alert-enricher`, Runtime: `Python 3.11`.
2. Tại mục **Configuration** → **Permissions**, gán execution role với các quyền: `s3:PutPublicAccessBlock`, `s3:GetBucketPolicy`, `s3:DeleteBucketPolicy`, `iam:AttachUserPolicy`, `iam:PutUserPolicy`, `dynamodb:PutItem`, `sns:Publish`.
3. Dán mã xử lý Python từ file `terraform/lambda/lambda_function.py`.

##### Bước 5: Tạo Bộ Phối hợp AWS Step Functions State Machine
1. Mở **AWS Step Functions Console** → Nhấn **Create state machine**. Chọn **Blank** (Workflow Studio).
2. Tên: `soc-detection-orchestrator`.
3. Kéo tác vụ Lambda `soclab-alert-enricher` vào biểu đồ workflow. Định nghĩa các trạng thái: `Detect → Enrich → Decide → Remediate → Notify → Log`.

##### Bước 6: Cấu hình Quy tắc Điều hướng Amazon EventBridge Rule
1. Mở **Amazon EventBridge Console** → Chọn **Rules** → Nhấn **Create rule**.
2. Tên quy tắc: `soc-cloudtrail-threat-rule`. Event Pattern:
   ```json
   {
     "source": ["aws.iam", "aws.s3"],
     "detail-type": ["AWS API Call via CloudTrail"],
     "detail": {
       "eventName": ["CreateUser", "AttachUserPolicy", "PutBucketPolicy", "DeleteBucketPolicy"]
     }
   }
   ```
3. Target: Chọn **Step Functions state machine** → `soc-detection-orchestrator`.

##### Bước 7: Kích hoạt AWS Security Hub & Scoped AWS Config Recorder
1. Mở **AWS Security Hub Console** → Nhấn **Enable Security Hub**. Bật tiêu chuẩn **CIS AWS Foundations Benchmark v1.4.0**.
2. Mở **AWS Config Console** → Chọn **Settings**. Rút gọn phạm vi recorder ghi 4 loại tài nguyên: `AWS::S3::Bucket`, `AWS::IAM::User`, `AWS::IAM::Role`, `AWS::IAM::Policy` với tần suất `Continuous`.

##### Bước 8: Cấu hình Amazon Athena Workgroup & Bảng Glue Catalog
1. Mở **Amazon Athena Console** → **Workgroups** → Nhấn **Create workgroup**. Tên: `soc_workgroup`. Vị trí lưu kết quả: `s3://soc-cloudtrail-logs-[MÃ_TÀI_KHOẢN_CỦA_BẠN]/athena-results/`.
2. Mở **Athena Query Editor** → Thực thi câu lệnh DDL từ `terraform/athena.tf` để tạo bảng `cloudtrail_logs`.

---

#### Kiểm tra & Xác nhận Tài nguyên qua AWS CLI & Console

##### 1. Kiểm tra Trạng thái AWS CloudTrail
```bash
aws cloudtrail describe-trails --query "trailList[*].[Name,Status,IsMultiRegionTrail]" --output table
```
![AWS CloudTrail Console Dashboard](/images/5-Workshop/5.3-Deploy-IaC/aws_cloudtrail_console.png)

##### 2. Kiểm tra Danh sách Bucket Amazon S3
```bash
aws s3 ls
```
![Amazon S3 Buckets Console](/images/5-Workshop/5.3-Deploy-IaC/aws_s3_buckets_console.png)

##### 3. Kiểm tra Hàng chờ Amazon SQS Queue
```bash
aws sqs list-queues
```
![Amazon SQS Console Dashboard](/images/5-Workshop/5.3-Deploy-IaC/aws_sqs_console.png)

##### 4. Kiểm tra Quy tắc Amazon EventBridge Rules
```bash
aws events list-rules --name-prefix "soc-"
```
![Amazon EventBridge Rules Console](/images/5-Workshop/5.3-Deploy-IaC/aws_eventbridge_rules.png)

##### 5. Kiểm tra State Machine AWS Step Functions
```bash
aws stepfunctions list-state-machines --query "stateMachines[*].[name,stateMachineArn,status]" --output table
```
![AWS Step Functions Console State Machine](/images/5-Workshop/5.3-Deploy-IaC/aws_step_functions_console.png)

##### 6. Kiểm tra Hàm AWS Lambda Function
```bash
aws lambda get-function --function-name soclab-alert-enricher --query "Configuration.[FunctionName,Runtime,State]"
```
![AWS Lambda Function Console](/images/5-Workshop/5.3-Deploy-IaC/aws_lambda_console.png)

##### 7. Kiểm tra Detector AWS GuardDuty
```bash
aws guardduty list-detectors
```
![AWS GuardDuty Settings Console](/images/5-Workshop/5.3-Deploy-IaC/aws_guardduty_console.png)

##### 8. Kiểm tra Bảng Audit Amazon DynamoDB Table
```bash
aws dynamodb describe-table --table-name automation-pipeline-events --query "Table.[TableName,TableStatus,ItemCount]"
```
![Amazon DynamoDB Table Console](/images/5-Workshop/5.3-Deploy-IaC/aws_dynamodb_console.png)
