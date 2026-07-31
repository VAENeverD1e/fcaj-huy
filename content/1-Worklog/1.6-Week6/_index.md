---
title: "Week 6 Worklog"
date: 2024-01-01
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---
### Week 6 Objectives

* Understand Amazon DynamoDB NoSQL key-value database concepts: partition keys, sort keys, secondary indexes, read/write capacity modes (Pay-Per-Request / Provisioned Free Tier), and Time To Live (TTL).
* Extend the Lambda enrichment function to store all triggered security alert records into DynamoDB for audit trail logging and UI dashboard querying.
* Master Amazon CloudWatch Monitoring: Custom Metrics, Metric Filters, CloudWatch Alarms, and notification triggers.
* Establish operational monitoring and cost guardrails for the serverless automation pipeline.

### Tasks Carried Out This Week

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| Mon | - Deep dive into Amazon DynamoDB NoSQL architecture.<br>- Provision DynamoDB table (`SecurityAlerts`) with `AlertID` (Partition Key) and `Timestamp` (Sort Key).<br>- Enable Time To Live (TTL) attribute (`ttl_expiry`) set to 90 days for automatic free-tier storage retention management. | 07/20/2026 | 07/20/2026 | <https://000060.awsstudygroup.com> |
| Tue | - Update Lambda function (`soc-alert-enricher`) using Boto3 `dynamodb.put_item()` to record every processed alert into the `SecurityAlerts` table.<br>- Include rich metadata: severity score, attacker IP, AWS account ID, raw event JSON, and remediation advice. | 07/21/2026 | 07/21/2026 | <https://000060.awsstudygroup.com> |
| Wed | - Study Amazon CloudWatch Metrics, Alarms, and Metric Filters.<br>- Create CloudWatch Metric Filter over SQS queue log streams to monitor message processing rates and DLQ counts. | 07/22/2026 | 07/22/2026 | <https://000008.awsstudygroup.com> |
| Thu | - Configure CloudWatch Alarms:<br>&emsp;+ `SQSQueueDepthAlarm`: Triggers if SQS queue depth exceeds 500 messages (detecting SIEM ingestion lag).<br>&emsp;+ `LambdaErrorAlarm`: Triggers on Lambda function execution failures.<br>&emsp;+ `FreeTierBudgetAlarm`: Monitors forecasted monthly billing. | 07/23/2026 | 07/23/2026 | <https://000036.awsstudygroup.com> |
| Fri | - Test parallel dual-alerting architecture: verified that attack simulations simultaneously trigger instant SNS alerts, write persistent DynamoDB log records, and ingest into Elastic SIEM.<br>- Verify metrics in CloudWatch console. | 07/24/2026 | 07/24/2026 | End-to-End Pipeline Testing |

### Week 6 Achievements

* Gained practical experience in NoSQL database design with Amazon DynamoDB and automated data lifecycle retention (TTL).
* Built a persistent security alert datastore recording enriched threat telemetry in DynamoDB.
* Configured CloudWatch Alarms ensuring operational health monitoring for Lambda execution and SQS queue depths.
* Validated the complete dual-alerting architecture: near-real-time serverless path (~2s push) running alongside deep SIEM correlation (~5min poll).
* Kept all resources strictly within AWS Free Tier limits (25 GB free DynamoDB storage, 10 free CloudWatch alarms).
