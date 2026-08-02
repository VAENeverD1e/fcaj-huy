---
title: "Week 4 Worklog"
date: 2026-07-06
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---
### Week 4 Objectives

* Analyze CloudTrail JSON log event schemas (`userIdentity`, `eventName`, `eventSource`, `requestParameters`, `sourceIPAddress`).
* Learn cloud security attack vectors, MITRE ATT&CK Cloud Matrix.
* Execute 5 realistic AWS cloud attack scenario simulations using a dedicated simulation IAM identity (`lab-attacker`).
* Engineer custom KQL detection rules in Elastic SIEM to detect cloud adversary behavior in real time.
* Verify automated cleanup routines to revert lab infrastructure immediately post-simulation.

### Tasks Carried Out This Week

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| Mon | - Study MITRE ATT&CK for Cloud.<br>- Create non-privileged simulation IAM user (`lab-attacker`) and generate access keys for attack execution.<br>- Deep dive into CloudTrail event structure for authentication and API calls. | 07/06/2026 | 07/06/2026 | <https://000044.awsstudygroup.com><br>MITRE ATT&CK Cloud Matrix |
| Tue | - Execute Cloud Attack Scenario 8: AWS Root Account Console Login without MFA.<br>- Execute Cloud Attack Scenario 9: IAM Privilege Reconnaissance & Policy Enumeration (`GetAccountSummary`, `ListUsers`, `ListRoles`). | 07/07/2026 | 07/07/2026 | <https://000011.awsstudygroup.com> |
| Wed | - Execute Cloud Attack Scenario 10: IAM Backdoor Persistence (creating rogue access keys `CreateAccessKey` and attaching admin policies `AttachUserPolicy`).<br>- Execute Cloud Attack Scenario 11: Bulk S3 Data Exfiltration (`GetObject` enumeration across sensitive buckets). | 07/08/2026 | 07/08/2026 | <https://000069.awsstudygroup.com> |
| Thu | - Execute Cloud Attack Scenario 12: Public S3 Bucket Exposure (modifying bucket ACL / removing Block Public Access via `PutBucketAcl` / `DeletePublicAccessBlock`).<br>- Run immediate lab cleanup scripts to revert permissions. | 07/09/2026 | 07/09/2026 | <https://000030.awsstudygroup.com> |
| Fri | - Query ingested CloudTrail logs in Elastic SIEM to locate exact API audit signatures.<br>- Develop, test, and tune 5 custom KQL rules targeting root logins, IAM recon, rogue credentials, S3 exfiltration, and public bucket exposure. | 07/10/2026 | 07/10/2026 | Elastic Security Rule Engine |

### Week 4 Achievements

* Gained clear understanding of AWS CloudTrail JSON audit event structures and API signatures.
* Successfully executed 5 cloud attack simulations covering authentication abuse, privilege recon, persistence, and data exfiltration.
* Authored 5 high-fidelity custom KQL rules in Elastic SIEM with low false-positive rates.
* Verified lab cleanup procedures to maintain zero security exposure post-testing.
* Established a fully functional hybrid detection engine covering both Windows endpoints and AWS cloud infrastructure.
