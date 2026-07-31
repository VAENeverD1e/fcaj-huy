---
title: "Week 8 Worklog"
date: 2024-01-01
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---
### Week 8 Objectives

* Understand AWS GuardDuty managed threat detection concepts: threat intelligence feeds, machine learning baseline anomaly detection, S3 Protection, and GuardDuty finding types.
* Enable a tightly scoped AWS GuardDuty detector under the 30-day free trial period with automated calendar milestones for disablement prior to trial expiry.
* Integrate GuardDuty findings with EventBridge to forward managed finding JSON payloads into the serverless alert pipeline.
* Re-run all 5 AWS cloud attack scenarios to evaluate GuardDuty findings against custom-engineered KQL detection rules.
* Author a comparative evaluation report (`guardduty-comparison.md`) assessing detection latency, visibility coverage, and cost posture.

### Tasks Carried Out This Week

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| Mon | - Study AWS GuardDuty architecture, finding types (`Recon:IAMUser`, `Persistence:IAMUser`, `Exfiltration:S3`), and S3 Protection mechanisms.<br>- Enable GuardDuty detector in trial mode with strict calendar reminder for disablement. | 08/03/2026 | 08/03/2026 | <https://000098.awsstudygroup.com> |
| Tue | - Create EventBridge Event Rule filtering GuardDuty finding events (`aws.guardduty`).<br>- Target Lambda function to parse and enrich GuardDuty managed finding JSON payloads for SNS/DynamoDB delivery. | 08/04/2026 | 08/04/2026 | <https://000018.awsstudygroup.com> |
| Wed | - Re-execute AWS Cloud Attack Scenarios 8–12:<br>&emsp;+ Root login without MFA<br>&emsp;+ IAM recon & enumeration<br>&emsp;+ IAM persistence key creation<br>&emsp;+ S3 exfiltration & public bucket exposure.<br>- Track detection timestamps across both Elastic SIEM (KQL) and GuardDuty. | 08/05/2026 | 08/05/2026 | Cloud Attack Simulation Suite |
| Thu | - Collect empirical comparison data:<br>&emsp;+ Custom KQL Latency: ~4–13 min SIEM ingestion latency.<br>&emsp;+ GuardDuty Baseline Limitations: Because GuardDuty was enabled late (26/07), only ~3 days elapsed before testing (29–30/07). Consequently, GuardDuty lacked sufficient baseline learning time for anomaly-based detectors (IAM recon, IAM persistence, S3 exfiltration) to fire, whereas its signature-based detector (`Policy:S3/BucketAnonymousAccessGranted`) alerted rapidly in ~6.7 min.<br>&emsp;+ Cost Posture: Custom rules rely on Free Tier SQS/Lambda ($0); GuardDuty converts to paid billing post-trial. | 08/06/2026 | 08/06/2026 | Empirical Testing Data |
| Fri | - Author comparative evaluation report (`guardduty-comparison.md`).<br>- Safely disable GuardDuty detector and S3 Protection prior to trial expiration to preserve $0 AWS spend. | 08/07/2026 | 08/07/2026 | <https://000098.awsstudygroup.com> |

### Week 8 Achievements

* Successfully integrated AWS GuardDuty managed threat detection into the serverless alert pipeline via EventBridge.
* Conducted a head-to-head empirical evaluation of custom KQL detection rules versus AWS GuardDuty managed findings.
* Evaluated GuardDuty detection timing: Documented that because GuardDuty was enabled late (26/07), machine-learning anomaly detectors lacked sufficient baseline history to fire on behavioral attack scenarios, highlighting the essential role of custom KQL SIEM rules for immediate zero-day alerting without baseline dependency.
* Authored the comprehensive `guardduty-comparison.md` analytical report.
* Exercised strict cost control by manually turning off GuardDuty before trial expiration, maintaining $0 AWS project spend.
