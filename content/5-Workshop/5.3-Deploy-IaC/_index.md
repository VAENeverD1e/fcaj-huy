---
title: "Deploying Infrastructure with Terraform"
date: 2026-07-29
weight: 3
chapter: false
pre: " <b> 5.3. </b> "
---

# 5.3 Infrastructure as Code Deployment with Terraform & AWS Verification

#### Overview

In this module, you will provision the entire AWS cloud side of the SOC Detection Lab using **Terraform Infrastructure as Code (IaC)**, examine actual HCL code modules, and verify each provisioned resource directly within both the **AWS Management Console** and **AWS CLI**.

---

#### Architecture Code Structure

The Terraform configuration lives in the `terraform/` directory:

```
terraform/
├── main.tf                 # Provider definitions and global settings
├── variables.tf            # Configurable inputs (region, bucket names, environment tags)
├── outputs.tf              # Exported resource ARNs and deployment endpoints
├── cloudtrail.tf           # Multi-region CloudTrail, S3 log bucket, KMS encryption
├── s3.tf                   # S3 Bucket policy and SQS event notification configuration
├── sqs.tf                  # SQS Dead-Letter Queue & SQS Queue for Elastic Agent polling
├── eventbridge.tf          # EventBridge Rules targeting Step Functions state machine
├── step_functions.tf       # AWS Step Functions state machine orchestrator (Detect -> Enrich -> Decide -> Remediate)
├── lambda.tf               # AWS Lambda function for alert processing & auto-remediation
├── lambda_remediation.tf   # Tightly scoped IAM permissions for Lambda remediation execution
├── athena.tf               # Athena Workgroup & Glue Catalog table DDL for CloudTrail SQL hunting
├── guardduty.tf            # GuardDuty Foundational Detector & S3 Protection setup
├── security_hub.tf         # Security Hub account enablement & scoped AWS Config recorder (CIS Benchmark)
├── dynamodb.tf             # DynamoDB telemetry table for automation audit logging
├── sns.tf                  # SNS alert topic and subscription endpoint
└── cloudwatch.tf           # Pipeline health metric alarms
```

---

#### Key Infrastructure Code Snippets

##### 1. CloudTrail & S3 Log Bucket Setup (`cloudtrail.tf`)

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

##### 2. EventBridge Threat Pattern Rule (`eventbridge.tf`)

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

##### 3. Lambda Alert Processor & IAM Role (`lambda.tf`)

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

##### 4. SQS Ingestion Queue for Elastic Fleet (`sqs.tf`)

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

---

#### Path A: Automated Infrastructure Deployment via Terraform CLI

Follow these exact terminal steps to provision all resources automatically:

1. **Navigate to the Terraform Directory**:

   ```bash
   cd terraform/
   ```

2. **Configure Variable Overrides (`terraform.tfvars`)**:
   Create a `terraform.tfvars` file:

   ```hcl
   aws_region          = "ap-southeast-2"
   environment         = "production"
   discord_webhook_url = "https://discord.com/api/webhooks/YOUR_WEBHOOK_URL"
   ```

3. **Initialize Terraform Modules & Providers**:

   ```bash
   terraform init
   ```

4. **Validate & Review Infrastructure Plan**:

   ```bash
   terraform plan -out=tfplan
   ```

5. **Apply Deployment**:

   ```bash
   terraform apply tfplan
   ```

![Terraform Deployment Execution Terminal Output](/images/5-Workshop/5.3-Deploy-IaC/terraform_apply.png)

---

#### Path B: Manual AWS Console Setup Guide (Step-by-Step)

If you prefer deploying resources manually to understand the architecture logic:

##### Step 1: Create Amazon S3 Central Log Bucket & SQS Queue

1. Open **Amazon S3 Console** → Click **Create bucket**. Name: `soc-cloudtrail-logs-[YOUR_ACCOUNT_ID]`, Region: `ap-southeast-2`. Enable **SSE-S3 encryption**.
2. Open **Amazon SQS Console** → Click **Create queue**. Type: Standard. Name: `soc-cloudtrail-sqs`. Configure Visibility Timeout: `300` seconds.

##### Step 2: Configure AWS CloudTrail Trail

1. Open **AWS CloudTrail Console** → Select **Trails** → Click **Create trail**.
2. Name: `soc-detection-lab-trail`. Storage: Select existing S3 bucket `soc-cloudtrail-logs-[YOUR_ACCOUNT_ID]`.
3. Enable **Multi-region trail** and **Management events** (`Read/Write: All`).

##### Step 3: Create Amazon DynamoDB Telemetry Table

1. Open **Amazon DynamoDB Console** → Click **Create table**.
2. Table name: `automation-pipeline-events` (must use hyphens). Partition key: `event_id` (String `S`). Read/Write capacity: On-Demand.

##### Step 4: Create AWS Lambda Auto-Remediation Function

1. Open **AWS Lambda Console** → Click **Create function**. Function name: `soclab-alert-enricher`, Runtime: `Python 3.11`.
2. In **Configuration** → **Permissions**, attach execution role with permissions: `s3:PutPublicAccessBlock`, `s3:GetBucketPolicy`, `s3:DeleteBucketPolicy`, `iam:AttachUserPolicy`, `iam:PutUserPolicy`, `dynamodb:PutItem`, `sns:Publish`.
3. Paste Python handler code from `terraform/lambda/lambda_function.py`.

##### Step 5: Create AWS Step Functions State Machine

1. Open **AWS Step Functions Console** → Click **Create state machine**. Choose **Blank** (Workflow Studio).
2. Name: `soc-detection-orchestrator`.
3. Drag Lambda task `soclab-alert-enricher` into the graph. Define execution states: `Detect → Enrich → Decide → Remediate → Notify → Log`.

##### Step 6: Configure Amazon EventBridge Rule

1. Open **Amazon EventBridge Console** → Select **Rules** → Click **Create rule**.
2. Rule name: `soc-cloudtrail-threat-rule`. Event Pattern:

   ```json
   {
     "source": ["aws.iam", "aws.s3"],
     "detail-type": ["AWS API Call via CloudTrail"],
     "detail": {
       "eventName": ["CreateUser", "AttachUserPolicy", "PutBucketPolicy", "DeleteBucketPolicy"]
     }
   }
   ```

3. Target: Select **Step Functions state machine** → `soc-detection-orchestrator`.

##### Step 7: Enable AWS Security Hub & Scoped AWS Config Recorder

1. Open **AWS Security Hub Console** → Click **Enable Security Hub**. Enable **CIS AWS Foundations Benchmark v1.4.0**.
2. Open **AWS Config Console** → Select **Settings**. Scope recorder to resource types: `AWS::S3::Bucket`, `AWS::IAM::User`, `AWS::IAM::Role`, `AWS::IAM::Policy` with **Continuous** delivery.

##### Step 8: Configure Amazon Athena Workgroup & Glue Catalog Table

1. Open **Amazon Athena Console** → **Workgroups** → Click **Create workgroup**. Name: `soc_workgroup`. Query result location: `s3://soc-cloudtrail-logs-[YOUR_ACCOUNT_ID]/athena-results/`.
2. Open **Athena Query Editor** → Execute DDL statement from `terraform/athena.tf` to create table `cloudtrail_logs`.

---

#### Step-by-Step AWS CLI & Console Verification

##### 1. AWS CloudTrail Status Verification

```bash
aws cloudtrail describe-trails --query "trailList[*].[Name,Status,IsMultiRegionTrail]" --output table
```

![AWS CloudTrail Console Dashboard](/images/5-Workshop/5.3-Deploy-IaC/aws_cloudtrail_console.png)

##### 2. Amazon S3 Storage Buckets Verification

```bash
aws s3 ls
```

![Amazon S3 Buckets Console](/images/5-Workshop/5.3-Deploy-IaC/aws_s3_buckets_console.png)

##### 3. Amazon SQS Queue Verification

```bash
aws sqs list-queues
```

![Amazon SQS Console Dashboard](/images/5-Workshop/5.3-Deploy-IaC/aws_sqs_console.png)

##### 4. Amazon EventBridge Rules Verification

```bash
aws events list-rules --name-prefix "soc-"
```

![Amazon EventBridge Rules Console](/images/5-Workshop/5.3-Deploy-IaC/aws_eventbridge_rules.png)

##### 5. AWS Step Functions State Machine Verification

```bash
aws stepfunctions list-state-machines --query "stateMachines[*].[name,stateMachineArn,status]" --output table
```

![AWS Step Functions Console State Machine](/images/5-Workshop/5.3-Deploy-IaC/aws_step_functions_console.png)

##### 6. AWS Lambda Function Verification

```bash
aws lambda get-function --function-name soclab-alert-enricher --query "Configuration.[FunctionName,Runtime,State]"
```

![AWS Lambda Function Console](/images/5-Workshop/5.3-Deploy-IaC/aws_lambda_console.png)

##### 7. AWS GuardDuty Detector Verification

```bash
aws guardduty list-detectors
```

![AWS GuardDuty Settings Console](/images/5-Workshop/5.3-Deploy-IaC/aws_guardduty_console.png)

##### 8. Amazon DynamoDB Telemetry Table Verification

```bash
aws dynamodb describe-table --table-name automation-pipeline-events --query "Table.[TableName,TableStatus,ItemCount]"
```

![Amazon DynamoDB Table Console](/images/5-Workshop/5.3-Deploy-IaC/aws_dynamodb_console.png)
