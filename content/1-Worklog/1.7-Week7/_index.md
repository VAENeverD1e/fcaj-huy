---
title: "Week 7 Worklog"
date: 2026-07-27
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---
### Week 7 Objectives

* Learn Infrastructure as Code (IaC) principles, HashiCorp Configuration Language (HCL) syntax, and Terraform declarative workflow (`init`, `plan`, `apply`, `destroy`).
* Understand Terraform State management, local vs. remote backends, resource dependencies (`depends_on`), and input/output variables.
* Codify the entire AWS security cloud stack using Terraform modules (`terraform/` directory).
* Validate automated, single-command environment replication (`terraform apply`) for all cloud components.

### Tasks Carried Out This Week

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| Mon | - Study Terraform fundamentals, AWS Provider configuration, and HCL code structuring principles.<br>- Initialize Terraform project directory (`terraform/`) and configure `main.tf`, `variables.tf`, and `outputs.tf`. | 07/27/2026 | 07/27/2026 | <https://000102.awsstudygroup.com> |
| Tue | - Codify Storage & Logging Layer in Terraform:<br>&emsp;+ Amazon S3 bucket module (`s3.tf`) with server-side encryption, Block Public Access, and bucket policies.<br>&emsp;+ AWS CloudTrail module (`cloudtrail.tf`) configured for multi-region logging.<br>&emsp;+ Amazon SQS queue module (`sqs.tf`) with dead-letter queue. | 07/28/2026 | 07/28/2026 | <https://000037.awsstudygroup.com> |
| Wed | - Codify Serverless & Automation Layer in Terraform:<br>&emsp;+ Amazon EventBridge rules (`eventbridge.tf`) matching CloudTrail security event patterns.<br>&emsp;+ AWS Lambda function module (`lambda.tf`) including Python code packaging and IAM execution role.<br>&emsp;+ Amazon SNS topic module (`sns.tf`) and email/webhook subscriptions. | 07/29/2026 | 07/29/2026 | <https://000038.awsstudygroup.com> |
| Thu | - Codify NoSQL & Analytics Layer in Terraform:<br>&emsp;+ Amazon DynamoDB table (`dynamodb.tf`) with `AlertID` key and TTL configuration.<br>&emsp;+ Amazon Athena database & workgroup module (`athena.tf`).<br>&emsp;+ AWS IAM Access Analyzer module (`access_analyzer.tf`). | 07/30/2026 | 07/30/2026 | <https://000102.awsstudygroup.com> |
| Fri | - Test full lifecycle execution: run `terraform plan` to audit 20+ plan resources.<br>- Execute `terraform apply` to provision the entire cloud architecture in a clean AWS region within ~3 minutes.<br>- Verify successful provisioned state and run `terraform destroy` test. | 07/31/2026 | 07/31/2026 | Terraform IaC Validation |

### Week 7 Achievements

* Mastered Infrastructure as Code (IaC) engineering using Terraform and HashiCorp Configuration Language (HCL).
* Fully codified the entire AWS cloud security stack into modular, reusable Terraform scripts under `terraform/`.
* Replaced manual AWS Console configurations with 100% reproducible Infrastructure as Code.
* Demonstrated single-command cloud environment deployment (`terraform apply`) in under 3 minutes.
* Ensured secure Terraform state file management with sensitive parameter masking and git isolation (`.gitignore`).
