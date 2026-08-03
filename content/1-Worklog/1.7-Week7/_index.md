---
title: "Week 7 Worklog"
date: 2026-07-27
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---
### Week 7 Objectives

* Learn Infrastructure as Code (IaC) principles and HashiCorp Configuration Language (HCL) syntax.
* Codify the entire AWS security cloud stack using Terraform modules (`terraform/` directory: S3, CloudTrail, SQS, EventBridge, Lambda, SNS, DynamoDB, Athena, Access Analyzer).
* Validate single-command environment deployment (`terraform apply`) and teardown (`terraform destroy`) in a clean AWS region within ~3 minutes.
* Perform comprehensive end-to-end system testing across all 12 attack scenarios and conduct AWS financial audit verifying $0 total spend.
* Finalize all technical documentation, architecture diagrams (`architecture.png`, `cloud-architecture.png`), and officially submit all project deliverables prior to the deadline on **July 31, 2026**.

### Tasks Carried Out This Week

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| Mon | - Study Terraform fundamentals, AWS Provider configuration, and HCL code structuring principles.<br>- Initialize Terraform project directory (`terraform/`) and configure `main.tf`, `variables.tf`, and `outputs.tf`. | 07/27/2026 | 07/27/2026 | <https://000102.awsstudygroup.com> |
| Tue | - Codify Storage, Logging & Serverless Layers in Terraform:<br>&emsp;+ Modules for Amazon S3 (`s3.tf`), CloudTrail (`cloudtrail.tf`), and Amazon SQS (`sqs.tf`).<br>&emsp;+ Modules for EventBridge (`eventbridge.tf`), AWS Lambda (`lambda.tf`), and Amazon SNS (`sns.tf`). | 07/28/2026 | 07/28/2026 | <https://000037.awsstudygroup.com><br><https://000038.awsstudygroup.com> |
| Wed | - Codify NoSQL, Analytics & Security Scanner Layers in Terraform:<br>&emsp;+ Modules for Amazon DynamoDB (`dynamodb.tf`), Amazon Athena (`athena.tf`), and IAM Access Analyzer (`access_analyzer.tf`). | 07/29/2026 | 07/29/2026 | <https://000102.awsstudygroup.com> |
| Thu | - Execute `terraform apply` single-command deployment validation in a clean region.<br>- Run full end-to-end integration testing across all 12 attack scenarios (7 Endpoint + 5 Cloud) and perform AWS Financial Audit ($0 spend confirmation). | 07/30/2026 | 07/30/2026 | Terraform & System Test Suite |
| Fri | - Finalize technical documentation, embed high-resolution architecture diagrams, and perform Hugo build test (`hugo`).<br>- **Official Project Submission**: Successfully complete and submit all project deliverables and portfolio repository prior to the **31/07/2026** deadline. | 07/31/2026 | 07/31/2026 | Final Project Submission |

### Week 7 Achievements

* Mastered Infrastructure as Code (IaC) engineering using Terraform and HashiCorp Configuration Language (HCL).
* Fully codified the entire AWS cloud security stack into modular, reusable Terraform scripts under `terraform/`.
* Demonstrated single-command cloud environment deployment (`terraform apply`) in under 3 minutes.
* Validated end-to-end integration across all 12 security threat scenarios with dual alerting and confirmed 100% financial discipline ($0 spend).
* Successfully finalized, published, and officially submitted the entire project portfolio and SOC documentation portal on **July 31, 2026**.
