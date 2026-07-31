---
title: "Blog 3"
date: 2026-07-31
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---

# TERRAFORM IaC — TẠI SAO NÊN DÙNG INFRASTRUCTURE AS CODE CHO AWS SECURITY LAB?

Khi xây dựng một Security Lab trên AWS với hàng chục dịch vụ liên kết chặt chẽ với nhau (CloudTrail, S3, SQS, EventBridge, Lambda, SNS, DynamoDB, GuardDuty, CloudWatch...), việc cấu hình thủ công qua AWS Console không chỉ mất thời gian mà còn rất dễ gây ra lỗi cấu hình không nhất quán. **Terraform** giải quyết hoàn toàn vấn đề này bằng cách cho phép bạn mô tả toàn bộ hạ tầng dưới dạng code (Infrastructure as Code — IaC).

## Terraform là gì?

Terraform là một công cụ IaC mã nguồn mở của HashiCorp, sử dụng ngôn ngữ khai báo **HCL (HashiCorp Configuration Language)** để định nghĩa các tài nguyên Cloud. Thay vì click từng bước trên Console, bạn viết file `.tf`, chạy `terraform apply` và toàn bộ hạ tầng sẽ được tạo ra tự động, chính xác và có thể lặp lại.

## 3 Lợi ích cốt lõi khi dùng Terraform cho Security Lab

### 1. Reproducibility — Tái tạo hạ tầng trong vài phút

Với Terraform, toàn bộ Security Lab của bạn (từ IAM roles, EventBridge rules, đến cấu hình DynamoDB indexes) được lưu trong các file `.tf` có thể commit lên Git. Khi cần rebuild môi trường mới (ví dụ: khi thay đổi AWS account hoặc region), chỉ cần:

```bash
terraform init
terraform apply
```

Toàn bộ hạ tầng sẽ được tạo lại y hệt cấu hình ban đầu — không sót một resource nào.

### 2. Security & Least Privilege — Kiểm soát IAM Policy chặt chẽ

Khi định nghĩa IAM Policy trong Terraform, bạn có thể áp dụng nguyên tắc **Least Privilege** một cách có hệ thống và dễ review. Ví dụ, IAM Policy cho Lambda chỉ cấp đúng 3 quyền cần thiết:

```hcl
resource "aws_iam_policy" "lambda_execution" {
  policy = jsonencode({
    Statement = [
      {
        Effect   = "Allow"
        Action   = ["logs:CreateLogGroup", "logs:CreateLogStream", "logs:PutLogEvents"]
        Resource = "arn:aws:logs:${var.aws_region}:${local.account_id}:*"
      },
      {
        Effect   = "Allow"
        Action   = ["sns:Publish"]
        Resource = aws_sns_topic.cloudtrail_alerts.arn  # Chỉ cấp quyền cho topic cụ thể
      },
      {
        Effect   = "Allow"
        Action   = ["dynamodb:PutItem", "dynamodb:UpdateItem"]
        Resource = aws_dynamodb_table.automation_pipeline_events.arn
      }
    ]
  })
}
```

Khi toàn bộ IAM policy nằm trong code, team có thể **Code Review** cấu hình bảo mật như review code thông thường — một lợi thế rất lớn trong môi trường an ninh.

### 3. Dependency Management — Quản lý thứ tự tạo tài nguyên tự động

Các tài nguyên AWS thường phụ thuộc lẫn nhau theo thứ tự nhất định: S3 Bucket phải tạo trước thì SQS Queue Policy mới có thể tham chiếu ARN của nó, Lambda Permission phải có sau khi EventBridge Rule được tạo. Terraform tự động phát hiện và quản lý các phụ thuộc này qua cú pháp tham chiếu trực tiếp:

```hcl
resource "aws_s3_bucket_notification" "cloudtrail_notify" {
  bucket = aws_s3_bucket.cloudtrail.id  # Terraform tự hiểu: S3 bucket phải tạo trước

  queue {
    queue_arn = aws_sqs_queue.cloudtrail_notifications.arn  # SQS Queue cũng phải tạo trước
    events    = ["s3:ObjectCreated:*"]
  }

  depends_on = [aws_sqs_queue_policy.allow_s3]  # Explicit dependency khi cần
}
```

## Thực hành tốt (Best Practices) khi dùng Terraform cho Security Project

| Practice | Lý do |
|---|---|
| Tách module theo service (`s3.tf`, `lambda.tf`, ...) | Dễ đọc, dễ maintain |
| Dùng `variables.tf` cho mọi giá trị cấu hình | Không hardcode Account ID, Region |
| Commit `.tfstate` vào S3 remote backend | Tránh conflict khi nhiều người cùng làm việc |
| Dùng `terraform plan` trước mỗi `apply` | Preview thay đổi, tránh xóa nhầm tài nguyên |
| Bật `force_destroy = true` chỉ trong môi trường Lab | Không dùng trong production |

## Tổng kết

Terraform không chỉ là công cụ tiết kiệm thời gian — nó còn là một phần của **Security Posture** của dự án. Khi hạ tầng bảo mật được định nghĩa dưới dạng code, mọi thay đổi đều có thể được kiểm tra, review và audit một cách minh bạch. Đây là lý do tại sao Terraform đã trở thành tiêu chuẩn công nghiệp cho các dự án Cloud Security nghiêm túc.

**🔗 Tham khảo thêm:**
- [Terraform AWS Provider Documentation](https://registry.terraform.io/providers/hashicorp/aws/latest/docs)
- [Terraform Best Practices](https://www.terraform-best-practices.com/)
- [AWS Well-Architected Framework — Security Pillar](https://docs.aws.amazon.com/wellarchitected/latest/security-pillar/welcome.html)