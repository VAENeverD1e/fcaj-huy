---
title: "Workshop"
date: 2026-07-29
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# Cloud-Native Security Monitoring System & SOC Detection Lab

#### Overview

Welcome to the hands-on **Cloud-Native Security Monitoring System & SOC Detection Lab** technical workshop! This project represents a complete, publication-grade implementation of an enterprise AWS cloud threat detection engineering platform deployed on AWS infrastructure.

Designed to strictly fulfill and exceed all FCAJ project requirements, this workshop guides you through building an AWS-native Security Operations Center (SOC) threat detection and automated incident response pipeline. You will simulate real cloud adversary techniques mapped to the **MITRE ATT&CK framework**, write custom behavioral detection rules in **Elastic SIEM (KQL)**, deploy automated serverless orchestration & auto-remediation pipelines via **AWS EventBridge, Step Functions, Lambda, SNS, and DynamoDB**, execute **Athena Native SQL Threat Hunting**, manage compliance with **AWS Security Hub CIS Benchmark**, and evaluate AWS-managed ML threat detection capabilities (**AWS GuardDuty** and **IAM Access Analyzer**).

---

#### Target Architecture & Operational System

The lab implements an AWS-native detect-decide-act loop paired with an empirical SIEM detection comparison framework:

1. **AWS Cloud Native Detect-Decide-Act Loop (Primary System)**:
   - **CloudTrail API Actions / GuardDuty ML / Security Hub CIS Benchmark → EventBridge → Step Functions State Machine → AWS Lambda → SNS, DynamoDB & Automated Remediation**.
   - Sub-5 second automated decision and response (e.g., reverting public S3 bucket policies via `s3:PutPublicAccessBlock`, isolating backdoor IAM accounts with `SecurityDenyAll` policy containment) plus SQL threat hunting via **Amazon Athena** and primary **Operations Dashboard** monitoring.

2. **Dual-Ingestion & SIEM Detection Benchmark Track**:
   - **CloudTrail → S3 → SQS → Elastic Agent / Fleet → Elastic SIEM**.
   - AWS CloudTrail telemetry streams continuously into Elastic SIEM to evaluate managed cloud-native detection against custom KQL rules in a side-by-side benchmark (`detection-comparison.md`).

#### Workshop Modules

1. **[5.1 Workshop Overview & Architecture](5.1-Workshop-overview/)**: Deep-dive into architecture components, threat models, service selection technical rationale, IAM security design, and dual-speed pipeline mechanics.
2. **[5.2 Prerequisites & Environment Setup](5.2-Prerequiste/)**: Account requirements, tool installations (Terraform, AWS CLI, Python, Elastic Cloud), and zero-spend cost guardrail initialization.
3. **[5.3 Infrastructure as Code Deployment](5.3-Deploy-IaC/)**: Automated provisioning of CloudTrail, S3, SQS, EventBridge, Step Functions, Lambda, GuardDuty, Security Hub, Athena, and DynamoDB using Terraform HCL scripts.
4. **[5.4 Adversary Attack Simulation](5.4-Attack-Simulation/)**: Executing 5 MITRE ATT&CK AWS cloud API attack scenarios (IAM recon, backdoor user creation, privilege escalation, public S3 policy tampering, bulk exfiltration).
5. **[5.5 Threat Detection & Serverless Alerting](5.5-Detection-Alerting/)**: Deploying Python Lambda auto-remediation, Step Functions state machine verification, Athena SQL threat hunting (`athena-hunt-queries.md`), Ops Dashboard monitoring, and comparative analysis (`detection-comparison.md`).
6. **[5.6 Teardown & Personal Reflection](5.6-Cleanup/)**: Resource destruction with `terraform destroy`, zero-spend trial guardrails verification, personal engineering reflections, real technical challenges faced, and future roadmap.
