---
title: "Workshop Overview"
date: 2026-07-29
weight: 1
chapter: false
pre: " <b> 5.1. </b> "
---

# 5.1 Workshop Overview & System Architecture

#### Introduction & Project Context

Modern enterprise attack surfaces have expanded rapidly into the cloud. Attackers exploit leaked access keys, misconfigured IAM permissions, and exposed storage buckets to quietly navigate environments. Because individual cloud API calls (such as `ListUsers` or `PutBucketPolicy`) resemble routine administrative actions, effective defense requires **contextual behavioral detection** paired with **low-latency automated alerting**.

This workshop guides you step-by-step through building a dual-track Security Operations Center (SOC) detection lab using both the **AWS Management Console** and **Infrastructure as Code (IaC)** via Terraform. The system captures telemetry across both Windows endpoints and AWS cloud infrastructure, ingests logs into an **Elastic SIEM** cluster, routes high-priority alerts via **serverless automation**, and compares custom KQL detection rules against AWS-managed security services (**AWS GuardDuty** and **IAM Access Analyzer**).

---

#### Problem Statement & Objectives

* **Problem Statement**: Standard SIEM batch log ingestion can introduce a 5-to-15 minute delay before security analysts receive notifications for active cloud attacks (such as backdoor account creation or public bucket exposure).
* **Target Audience**: Cloud Security Engineers, SOC Analysts, and Cloud Engineers needing real-time incident response alongside deep historical correlation capabilities.
* **Core Goal & Deliverables**:
  1. Build an **AWS-native detect-decide-act loop (<5 seconds latency)** featuring Step Functions orchestration, Lambda auto-remediation (reverting public S3 policies, applying `SecurityDenyAll` containment to backdoor IAM users), and Security Hub CIS Benchmark compliance.
  2. Maintain continuous **AWS telemetry ingestion via S3/SQS into Elastic SIEM** for side-by-side detection benchmarking.
  3. Execute **5 MITRE ATT&CK AWS cloud attack scenarios** to produce an empirical comparison benchmark (`detection-comparison.md`).
  4. Perform **Athena Native SQL Threat Hunting** directly over S3 CloudTrail log archives (`athena-hunt-queries.md`).
  5. Enforce **100% Zero-Spend Cost Guardrails** on AWS.

---

#### High-Level System Architecture

![SOC Detection Lab System Architecture](/images/5-Workshop/5.1-Workshop-overview/architecture_diagram.png)

---

#### Service Selection Technical & Economic Rationale

To comply with the project evaluation rubric (Rubric 4.2), the technical and cost rationale for selecting each AWS component is detailed below:

| AWS Service | Selected Role | Alternatives Evaluated | Selection Rationale (Technical & Cost) |
| :--- | :--- | :--- | :--- |
| **AWS CloudTrail** | Multi-Region API Audit Ingestion | Custom API polling, VPC Flow Logs only | **Native Audit Trail**: Captures management and data events across all AWS services. Free Tier covers management events. |
| **Amazon S3** | Durable Central Log Repository | Direct HTTP streaming to SIEM | **Decoupled Durability**: Acts as an immutable landing zone. Encryption-at-rest (SSE-S3) enabled with zero idle compute costs. |
| **Amazon SQS** | Ingestion Queue & Decoupling | Direct S3 notification polling | **Backpressure & Reliability**: Decouples Elastic Fleet ingestion from S3. Prevents log loss during burst events and scales automatically. |
| **Amazon EventBridge** | Real-Time Threat Event Router | CloudWatch Logs subscription filter | **Sub-Second Pattern Matching**: Filters CloudTrail management events & GuardDuty findings instantaneously without polling. |
| **AWS Lambda** | Serverless Alert Processing & Webhook Delivery | Dedicated EC2 alert worker instance | **Zero Idle Cost & Automatic Scaling**: Executes Python alert logic only when triggered. Zero compute cost when idle ($0.00 spend). |
| **Amazon DynamoDB** | Operational Telemetry Audit Database | Relational Database (Amazon RDS) | **Low Latency & Serverless On-Demand**: Stores pipeline alert metadata and execution timestamps with zero idle database instance cost. |
| **Amazon SNS** | Multi-Channel Fanout Notification | Hardcoded HTTP requests only | **Fan-out Capability**: Allows alerting payloads to simultaneously notify Discord webhooks, email endpoints, and PagerDuty topics. |
| **AWS GuardDuty** | Managed ML Threat Detection | Manual rule maintenance only | **Anomalous ML Baselining**: Detects behavioral deviations (S3 exfiltration, API anomalies) that static rules miss. Evaluated in 30-day trial mode. |
| **IAM Access Analyzer** | Resource Policy Public Access Scanner | Manual policy auditing | **Automated Static Logic Proofs**: Evaluates S3 bucket policies continuously to detect unauthorized cross-account or public exposure. |

---

#### Security Architecture & IAM Design Principles

1. **Principle of Least Privilege (PoLP)**:
   * AWS Lambda execution role (`soc-lambda-execution-role`) is granted minimal permissions (`dynamodb:PutItem`, `sns:Publish`, `logs:CreateLogGroup`, `logs:PutLogEvents`).
   * SQS Queue access policy strictly limits `sqs:SendMessage` to the specific CloudTrail S3 bucket ARN using `aws:SourceArn` condition keys.
2. **Secrets & Credentials Safety**:
   * Zero access keys are hardcoded in Terraform code, Python scripts, or repository commits.
   * Discord webhook URLs are injected via Lambda environment variables encrypted at rest.
3. **Data Protection & Encryption**:
   * S3 CloudTrail log buckets enforce **Server-Side Encryption (SSE-S3)** and enable **Block Public Access** settings.
   * All network transit requires **TLS 1.3**.

---

#### System Scalability & Operational Health

* **Event-Driven Elasticity**: The serverless automation path automatically scales from 0 to thousands of events/sec without provisioning servers.
* **Queue Decoupling**: SQS absorbs spikes during heavy CloudTrail log volume, ensuring Elastic Agent pulls logs smoothly without overloading the SIEM cluster.
* **Operational Monitoring**: AWS CloudWatch alarms monitor Lambda execution errors (`Errors > 0`) and SQS Dead-Letter Queue (DLQ) messages (`ApproximateNumberOfMessagesVisible > 0`).

---

#### Telemetry Pipeline Latency & Capability Comparison

| Component | Ingestion Path | Latency | Primary Use Case |
| :--- | :--- | :--- | :--- |
| **Serverless Automation** | CloudTrail / GuardDuty → EventBridge → Lambda → SNS / Discord | **< 10 seconds** | Immediate notification of high-severity threat actions (e.g., backdoor creation, public S3 policy). |
| **Elastic SIEM Pipeline** | CloudTrail → S3 → SQS → Elastic Agent → Elastic SIEM | **~5 minutes** | Complex correlation, EQL sequence rules, historical threat hunting, and visual dashboards. |
| **Endpoint Telemetry** | Sysmon / Suricata → Elastic Agent → Elastic SIEM | **Near-real-time** | Process execution, LSASS memory dumping, network connection monitoring. |
