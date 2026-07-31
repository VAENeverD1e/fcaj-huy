---
title: "Week 5 Worklog"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---
### Week 5 Objectives

* Understand AWS Event-Driven Architecture principles: event producers, event buses, event rules, and targets.
* Master Amazon EventBridge rule creation using custom JSON event patterns matching CloudTrail security events.
* Learn AWS Lambda serverless execution models, execution roles, environment variables, and Python Boto3 SDK integration.
* Learn Amazon SNS messaging: topic creation, access policies, email/HTTP webhook subscriptions, and notification formatting.
* Architect a near-real-time serverless alert notification path: EventBridge → Lambda → SNS.

### Tasks Carried Out This Week

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| Mon | - Study AWS EventBridge architecture, custom event buses, and JSON event pattern syntax.<br>- Create Amazon SNS Topic (`soc-security-alerts-topic`) and subscribe email/webhook endpoints. | 07/13/2026 | 07/13/2026 | <https://000077.awsstudygroup.com> |
| Tue | - Write EventBridge Event Rules filtering critical CloudTrail API events:<br>&emsp;+ Root console logins without MFA<br>&emsp;+ IAM policy modifications (`AttachUserPolicy`, `PutUserPolicy`)<br>&emsp;+ Unauthorized access attempts (`UnauthorizedOperation`). | 07/14/2026 | 07/14/2026 | <https://000054.awsstudygroup.com> |
| Wed | - Learn AWS Lambda deployment, Python runtime environment, and IAM execution roles (`LambdaSOCAlertExecutionRole`).<br>- Write Lambda function (`soc-alert-enricher`) using Boto3 to parse incoming EventBridge event payloads. | 07/15/2026 | 07/15/2026 | <https://000022.awsstudygroup.com> |
| Thu | - Implement alert enrichment logic in Lambda: extracting caller IP, AWS account ID, user identity type, event timestamp, and API action.<br>- Configure Lambda to format human-readable alert messages and publish directly to the SNS Topic via Boto3. | 07/16/2026 | 07/16/2026 | <https://000022.awsstudygroup.com> |
| Fri | - Wire EventBridge rules to target the Lambda function.<br>- Trigger test cloud attack scenarios to verify real-time alert delivery.<br>- Benchmark delivery latency: verified near-real-time notification (~2–5 seconds). | 07/17/2026 | 07/17/2026 | Serverless Pipeline Testing |

### Week 5 Achievements

* Mastered event-driven architecture pattern using Amazon EventBridge and serverless AWS Lambda.
* Configured Amazon SNS messaging topics delivering instant security notifications.
* Engineered a Python Lambda function parsing and enriching raw CloudTrail audit JSON events.
* Established a sub-5-second serverless notification pipeline operating in parallel with deep SIEM log correlation (~5 min).
* Maintained 100% Free Tier compliance (well within 1M free Lambda requests and 100K SNS notifications per month).
