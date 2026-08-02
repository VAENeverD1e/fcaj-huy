---
title: "Week 3 Worklog"
date: 2026-06-29
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---
### Week 3 Objectives

* Learn Amazon S3 fundamentals: bucket creation, lifecycle policies, server-side encryption (SSE-S3), bucket policies, and access control lists (ACLs).
* Master AWS IAM advanced concepts: IAM Roles, Trust Relationships, Service Roles, Least Privilege policies, and AssumeRole API mechanisms.
* Understand AWS CloudTrail logging architecture: Management Events vs. Data Events, multi-region trail configuration, and log file integrity validation.
* Build the Cloud Infrastructure Ingestion Pipeline: AWS CloudTrail → Amazon S3 → Amazon SQS → Elastic Agent AWS Integration Fleet.

### Tasks Carried Out This Week

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| Mon | - Deep dive into Amazon S3 storage concepts & security controls.<br>- Create secure S3 bucket (`soc-cloudtrail-logs-lab`) with SSE-S3 encryption, Block Public Access enabled, and TLS-only enforcement policy. | 06/29/2026 | 06/29/2026 | <https://000057.awsstudygroup.com><br><https://000069.awsstudygroup.com> |
| Tue | - Study IAM Roles and Trust Relationships.<br>- Create least-privilege IAM policies and service roles (`ElasticAgentCloudTrailRole`) for secure log consumption without long-term credentials. | 06/30/2026 | 06/30/2026 | <https://000002.awsstudygroup.com><br><https://000048.awsstudygroup.com> |
| Wed | - Enable AWS CloudTrail across all regions targeting the S3 log bucket.<br>- Configure CloudTrail S3 Event Notifications to publish ObjectCreated events to an Amazon SQS queue (`soc-cloudtrail-sqs-queue`). | 07/01/2026 | 07/01/2026 | <https://cloudjourney.awsstudygroup.com/3-optimize/> |
| Thu | - Configure Amazon SQS queue policy to accept event notifications exclusively from the designated CloudTrail S3 bucket.<br>- Set up Dead Letter Queue (DLQ) for failed message handling. | 07/02/2026 | 07/02/2026 | <https://000077.awsstudygroup.com> |
| Fri | - Configure Elastic Agent Fleet AWS Integration (`aws-cloudtrail` data stream).<br>- Connect Elastic SIEM to the SQS queue endpoint.<br>- Verify end-to-end audit log stream in Elastic SIEM (`logs-aws.cloudtrail-*`). | 07/03/2026 | 07/03/2026 | Elastic AWS Integration Docs |

### Week 3 Achievements

* Acquired in-depth knowledge of Amazon S3 security hardening and IAM Role-based access control.
* Deployed a multi-region AWS CloudTrail logging architecture storing audit logs in encrypted S3 storage.
* Architected an asynchronous event-driven log forwarding pipeline using S3 Event Notifications and Amazon SQS.
* Successfully ingested AWS CloudTrail telemetry into Elastic SIEM for centralized cloud audit logging.
* Ensured total cost containment by remaining strictly within AWS Free Tier limits (management events and free SQS operations).
