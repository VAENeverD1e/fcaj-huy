---
title: "Week 7 Worklog"
date: 2024-01-01
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---
### Week 7 Objectives

* Understand AWS IAM Access Analyzer passive security scanning concepts: analyzer creation, trust zone evaluation, external access findings, and policy validation.
* Learn Amazon Athena serverless SQL query engine and its role in querying CloudTrail audit logs stored in Amazon S3.
* Enable IAM Access Analyzer to detect unintended external access to AWS resources (S3, IAM, Lambda, SQS) without incurring extra recording costs.
* Author a library of 4 core SOC forensic SQL queries in Amazon Athena Console for threat hunting over CloudTrail S3 logs.

### Tasks Carried Out This Week

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| Mon | - Study IAM Access Analyzer passive scanning capabilities.<br>- Enable IAM Access Analyzer with the account as the trust zone boundary.<br>- Review findings generated for external cross-account access and public S3 resource shares. | 07/27/2026 | 07/27/2026 | <https://000030.awsstudygroup.com> |
| Tue | - Learn Amazon Athena serverless SQL query concepts.<br>- Configure Athena query result output location in Amazon S3. | 07/28/2026 | 07/28/2026 | <https://000106.awsstudygroup.com> |
| Wed | - Explore AWS CloudTrail integration with Amazon Athena.<br>- Create Athena table over CloudTrail S3 log bucket using standard AWS console template. | 07/29/2026 | 07/29/2026 | <https://000040.awsstudygroup.com> |
| Thu | - Author and test 4 SOC forensic SQL queries in Athena Query Editor:<br>&emsp;+ Query 1: Top IP addresses making failed API calls (`errorCode IS NOT NULL`).<br>&emsp;+ Query 2: IAM access key creation timeline (`CreateAccessKey`).<br>&emsp;+ Query 3: S3 bucket policy changes (`PutBucketPolicy`/`DeleteBucketPolicy`).<br>&emsp;+ Query 4: Root user API actions executed within the last 7 days. | 07/30/2026 | 07/30/2026 | <https://000106.awsstudygroup.com> |
| Fri | - Review query execution results and execution times in Athena Console.<br>- Confirm zero additional standing infrastructure cost for Athena and IAM Access Analyzer. | 07/31/2026 | 07/31/2026 | AWS Cost Management |

### Week 7 Achievements

* Successfully enabled AWS IAM Access Analyzer for automated, passive misconfiguration detection (always free).
* Configured Amazon Athena Query Editor and created a CloudTrail log table using standard AWS console templates.
* Built a practical library of 4 forensic SQL queries enabling rapid threat hunting directly against raw S3 audit logs.
* Confirmed that passive scanning and on-demand Athena queries incur zero standing monthly costs.
