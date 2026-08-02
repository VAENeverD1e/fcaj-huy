---
title: "Week 6 Worklog"
date: 2026-07-20
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---
### Week 6 Objectives

* Understand IAM Access Analyzer passive scanning, Amazon Athena serverless SQL log queries, and AWS GuardDuty managed threat detection.
* Enable IAM Access Analyzer to detect external cross-account access and public resource shares.
* Build a library of 4 core SOC forensic SQL queries in Amazon Athena to hunt for threats directly over CloudTrail logs stored in S3.
* Enable AWS GuardDuty detector under the 30-day free trial period and evaluate managed findings against custom-engineered KQL detection rules.
* Author a comparative evaluation report (`guardduty-comparison.md`) assessing detection latency, visibility coverage, and cost posture, then safely disable GuardDuty before trial expiry.

### Tasks Carried Out This Week

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| Mon | - Enable IAM Access Analyzer with the AWS account as the trust zone boundary to audit public/cross-account access.<br>- Configure Amazon Athena database & workgroup over CloudTrail S3 log bucket. | 07/20/2026 | 07/20/2026 | <https://000030.awsstudygroup.com> |
| Tue | - Author and execute 4 SOC forensic SQL queries in Athena Query Editor:<br>&emsp;+ Failed API calls by IP<br>&emsp;+ Access key creation timelines<br>&emsp;+ S3 bucket policy changes<br>&emsp;+ Root user API actions in last 7 days. | 07/21/2026 | 07/21/2026 | <https://000106.awsstudygroup.com> |
| Wed | - Enable AWS GuardDuty detector in trial mode with strict calendar reminder for disablement.<br>- Create EventBridge Event Rule filtering GuardDuty findings (`aws.guardduty`) and forwarding JSON payloads to Lambda. | 07/22/2026 | 07/22/2026 | <https://000098.awsstudygroup.com> |
| Thu | - Re-execute AWS Cloud Attack Scenarios 8–12 (Root login without MFA, IAM recon, IAM persistence key, S3 exfiltration).<br>- Collect empirical comparison data: Custom KQL latency (~4–13 min) vs. GuardDuty baseline dependencies and detection timing. | 07/23/2026 | 07/23/2026 | Empirical Testing Data |
| Fri | - Author comparative evaluation report (`guardduty-comparison.md`).<br>- Safely disable GuardDuty detector and S3 Protection prior to trial expiration to maintain $0 AWS spend. | 07/24/2026 | 07/24/2026 | <https://000098.awsstudygroup.com> |

### Week 6 Achievements

* Successfully enabled AWS IAM Access Analyzer for passive misconfiguration detection at zero cost.
* Built a practical library of 4 forensic SQL queries in Amazon Athena enabling rapid threat hunting over S3 CloudTrail logs.
* Integrated AWS GuardDuty managed threat detection into the serverless alert pipeline via EventBridge.
* Conducted a head-to-head empirical evaluation of custom KQL detection rules vs. GuardDuty managed findings and authored `guardduty-comparison.md`.
* Exercised strict financial discipline by disabling GuardDuty before trial expiration, preserving $0 total AWS spend.
