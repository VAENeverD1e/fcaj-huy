---
title: "Week 5 Worklog"
date: 2026-07-13
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---
### Week 5 Objectives

* Understand AWS Event-Driven Architecture principles: event producers, event buses, event rules, and targets.
* Master Amazon EventBridge rule creation using custom JSON event patterns matching CloudTrail security events.
* Learn AWS Lambda serverless execution models, execution roles, and Python Boto3 SDK integration.
* Learn Amazon SNS messaging (email alerts) and Amazon DynamoDB NoSQL key-value datastore (table `SecurityAlerts` with TTL retention).
* Architect and validate a complete real-time serverless security pipeline: EventBridge → Lambda → SNS + DynamoDB datastore.

### Tasks Carried Out This Week

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| Mon | - Study AWS EventBridge architecture, custom event buses, and JSON event pattern syntax.<br>- Create Amazon SNS Topic (`soc-security-alerts-topic`) and subscribe email/webhook endpoints. | 07/13/2026 | 07/13/2026 | <https://000077.awsstudygroup.com> |
| Tue | - Write EventBridge Event Rules filtering critical CloudTrail API events (`RootNoMFA`, IAM policy changes, unauthorized operations).<br>- Provision Amazon DynamoDB table (`SecurityAlerts`) with `AlertID` partition key and 90-day TTL retention. | 07/14/2026 | 07/14/2026 | <https://000060.awsstudygroup.com> |
| Wed | - Develop Python AWS Lambda function (`soc-alert-enricher`) with IAM execution role.<br>- Implement Boto3 logic to enrich CloudTrail events and push real-time alerts to SNS Topic. | 07/15/2026 | 07/15/2026 | <https://000022.awsstudygroup.com> |
| Thu | - Extend Lambda function to persist enriched security alert records into DynamoDB (`dynamodb.put_item()`).<br>- Configure CloudWatch Alarms (`SQSQueueDepthAlarm`, `LambdaErrorAlarm`, `FreeTierBudgetAlarm`). | 07/16/2026 | 07/16/2026 | <https://000008.awsstudygroup.com> |
| Fri | - Wire EventBridge rules to target Lambda function.<br>- Execute Cloud attack simulations to verify dual alerting path: instant SNS email delivery (~2s) and persistent audit logging in DynamoDB. | 07/17/2026 | 07/17/2026 | Serverless Pipeline Testing |

### Week 5 Achievements

* Mastered event-driven architecture pattern using Amazon EventBridge, AWS Lambda, and Amazon SNS.
* Engineered a Python Lambda function that enriches CloudTrail audit events in real time and publishes push notifications.
* Built a persistent security alert datastore recording enriched threat telemetry directly into Amazon DynamoDB.
* Configured CloudWatch Alarms ensuring operational health monitoring for Lambda execution and queue depths.
* Validated sub-5-second serverless notification delivery while keeping all resources 100% within AWS Free Tier limits.
