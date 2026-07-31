---
title: "Clean Up Resources"
date: 2026-07-29
weight: 6
chapter: false
pre: " <b> 5.6. </b> "
---

# 5.6 Environment Teardown & Personal Reflection

#### Overview

To maintain a strict **Zero-Spend Budget posture** and prevent unexpected cloud charges post-workshop, follow this step-by-step cleanup procedure using both the **AWS Management Console** and **Terraform CLI**. Additionally, this section documents the **Personal Customizations** and **Engineering Reflections** developed during the project execution (in compliance with FCAJ Rubric 4.5).

---

#### 1. Step-by-Step Resource Destruction

##### Step 1: Empty S3 Log Buckets via AWS Console or CLI
CloudTrail log buckets and S3 event destination buckets cannot be deleted by Terraform if they contain objects.
1. Open **Amazon S3 Console**.
2. Select log & test buckets (`soc-cloudtrail-logs-*`, `soclab-exfil-test-demo`, `soclab-public-test-demo`).
3. Click **Empty** and confirm deletion of all log objects.

```bash
# CLI Empty Commands:
aws s3 rm s3://soc-cloudtrail-logs-demo-bucket --recursive
aws s3 rm s3://soclab-exfil-test-demo --recursive
aws s3 rm s3://soclab-public-test-demo --recursive
```

![Amazon S3 Empty Bucket Console](/images/5-Workshop/5.6-Cleanup/s3_empty_bucket_console.png)

##### Step 2: Destroy Terraform Provisioned Infrastructure
Navigate to the `terraform/` directory and execute teardown:
```bash
cd terraform/
terraform destroy -auto-approve
```

![Terraform Destroy Teardown Terminal Output](/images/5-Workshop/5.6-Cleanup/terraform_destroy_terminal.png)

##### Step 3: Disable AWS GuardDuty Detector in AWS Console
To prevent automatic trial-to-paid conversion after 30 days:
1. Open **AWS GuardDuty Console** → Select **Settings**.
2. Scroll to the bottom and select **Disable GuardDuty**.
3. Confirm detector deletion.

![AWS GuardDuty Disable Detector Console](/images/5-Workshop/5.6-Cleanup/guardduty_disable_console.png)

##### Step 4: Verify Zero Spend in AWS Cost Explorer
1. Open **AWS Billing and Cost Management Console** → **Cost Explorer**.
2. Confirm total billed spend for the month displays **$0.00**.

![AWS Cost Explorer Zero Spend Verification Console](/images/5-Workshop/5.6-Cleanup/aws_cost_explorer_zero_spend.png)

---

#### 2. Cost Guardrail Verification Checklist

- [x] Confirm `terraform destroy` completed with zero remaining cloud resources.
- [x] Verify AWS Cost Explorer shows **$0.00** total billed spend.
- [x] Confirm GuardDuty Detector status is **Disabled / Deleted**.
- [x] Confirm S3 Log Buckets are completely removed.
- [x] Confirm Zero-Spend Budget email alert remains active on the root account.

---

#### 3. Personal Customizations & Engineering Innovations (Rubric 4.5)

Rather than copying standard workshop templates, this lab introduced several custom engineering enhancements:

1. **Dual-Path Latency Architecture**:
   - Standard labs ingest all logs solely into a SIEM. We decoupled the pipeline into a **<10s Serverless Automation Path** (EventBridge → Lambda → Discord/DynamoDB) and a **~5min SIEM Correlation Path** (S3 → SQS → Elastic Fleet). This ensures high-risk attacks (like backdoor creation) alert instantly without waiting for batch SIEM ingestion.
2. **Operational Telemetry Database (DynamoDB)**:
   - Implemented an audit table (`automation-pipeline-events`) to record every processed serverless alert, enabling pipeline health monitoring and auditability.
3. **EQL Sequence Correlation Rules**:
   - Engineered stateful EQL rules (`sequence by actor with maxspan=5m`) to correlate multi-step cloud attacks (e.g., `CreateUser` followed by `AttachUserPolicy`) rather than relying solely on single-event triggers.

---

#### 4. Personal Reflections & Real Challenges Faced

Building a full SOC detection pipeline from scratch was a huge learning journey. Coming into this project, I ran into several personal and technical hurdles:

- **Navigating the AWS Ecosystem & Service Selection**:
  - *The Challenge*: AWS has hundreds of services, and at first, I wasn't sure which ones were right for my pipeline. Figuring out the differences and how to connect EventBridge, SQS, Step Functions, and Lambda took a lot of reading and trial-and-error.
- **Fear of Unexpected Cloud Billing**:
  - *The Challenge*: Working on cloud infrastructure, my biggest fear was accidentally leaving a paid resource running or exceeding Free Tier limits and waking up to a surprise bill.
  - *How I Handled It*: I strictly monitored AWS Budgets, used Free Tier-eligible configurations, set up zero-spend billing alerts, and tracked every resource in Terraform so I could tear them down cleanly when done.
- **Trial-and-Error with Misconfigurations**:
  - *The Challenge*: I hit plenty of real-world roadblocks — IAM permission errors (`AccessDenied`), Terraform syntax mistakes, event pattern matching issues, and region mismatches between global IAM events (`us-east-1`) and regional services (`ap-southeast-2`).
  - *How I Handled It*: I spent hours digging through CloudWatch logs, reading AWS documentation, and troubleshooting IAM policies step-by-step until the data flowed smoothly.
- **Assembling a Large End-to-End Detection Pipeline**:
  - *The Challenge*: Connecting so many moving parts — CloudTrail, SQS, EventBridge, Lambda, Step Functions, DynamoDB, Elastic SIEM, and Athena — into a single coherent pipeline felt daunting at the start.
  - *The Takeaway*: Breaking the architecture down into small, bite-sized steps (logging → ingestion → automation → SIEM) helped me manage the complexity and build confidence in cloud engineering.

---

#### 5. Common Troubleshooting & Re-creation Pitfalls Guide

| Symptom / Error Message | Root Cause | Exact Fix & Resolution |
| :--- | :--- | :--- |
| `ResourceNotFoundException: Requested resource not found` (DynamoDB) | DynamoDB table name typo (e.g. `automation_pipeline_events` instead of `automation-pipeline-events`). | Ensure table name uses hyphens (`automation-pipeline-events`) and partition key is `event_id` (`S`). |
| CloudTrail Console Error: `"The lookup attribute value is not valid"` | Searching for IAM events in `ap-southeast-2` or querying with `eventId` lowercase. | Global IAM actions (`CreateUser`, `AttachUserPolicy`) emit in `us-east-1`. Use `EventName=${action}` parameter and query `us-east-1` for IAM events and `ap-southeast-2` for S3 events. |
| `HIVE_CURSOR_ERROR: Cannot deserialize JSON` in Athena | CloudTrail JSON containing malformed records or unexpected struct schemas. | Ensure table DDL in `athena.tf` uses `org.openx.data.jsonserde.JsonSerDe` with `TBLPROPERTIES ("ignore.malformed.json" = "true")`. |
| `BucketNotEmpty` error during `terraform destroy` | CloudTrail log objects remain inside S3 log buckets. | Empty S3 buckets first via CLI (`aws s3 rm s3://[bucket-name] --recursive`) before running `terraform destroy`. |
| AWS Config Recorder charges exceeding Free Tier | Config recorder set to record all global resource types continuously. | Scope Config recorder in `security_hub.tf` exclusively to `AWS::S3::Bucket`, `AWS::IAM::User`, `AWS::IAM::Role`, and `AWS::IAM::Policy`. |