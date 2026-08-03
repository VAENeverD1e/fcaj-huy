---
title: "Week 6 Worklog"
date: 2026-07-20
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---
### Week 6 Objectives

* Understand IAM Access Analyzer passive scanning, Amazon Athena serverless SQL log queries, and AWS GuardDuty managed threat detection.
* Build a library of 4 core SOC forensic SQL queries in Amazon Athena to hunt for threats directly over CloudTrail logs stored in S3.
* Evaluate AWS GuardDuty managed findings against custom KQL detection rules and author comparative report (`guardduty-comparison.md`).
* Build interactive frontend web dashboard UI (`frontend/`) using React, Vite, and modern dark-theme CSS design system.
* Wire React UI widgets (`PipelineStatusWidget`, `CostTrackerWidget`, `LatencyChart`, `AlertFeedTable`) to Python REST API endpoints with automated 30-second telemetry polling.

### Tasks Carried Out This Week

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| Mon | - Enable IAM Access Analyzer with the AWS account as trust zone boundary.<br>- Configure Amazon Athena database & workgroup over CloudTrail S3 log bucket. | 07/20/2026 | 07/20/2026 | <https://000030.awsstudygroup.com> |
| Tue | - Author and execute 4 SOC forensic SQL queries in Athena Query Editor (failed API calls, access key timelines, bucket policy changes, root actions).<br>- Enable AWS GuardDuty detector in 30-day trial mode. | 07/21/2026 | 07/21/2026 | <https://000106.awsstudygroup.com> |
| Wed | - Collect empirical comparison data: Custom KQL latency (~4–13 min) vs. GuardDuty baseline timing.<br>- Author comparative evaluation report (`guardduty-comparison.md`) and safely disable GuardDuty detector to preserve $0 AWS spend. | 07/22/2026 | 07/22/2026 | <https://000098.awsstudygroup.com> |
| Thu | - Initialize React application project using Vite (`npx create-vite@latest frontend --template react`).<br>- Configure modern CSS design system, dark mode palette, and build UI components (`PipelineStatusWidget`, `CostTrackerWidget`, `LatencyChart`, `AlertFeedTable`). | 07/23/2026 | 07/23/2026 | <https://000079.awsstudygroup.com> |
| Fri | - Wire React frontend to Python REST API endpoints (`http://localhost:5000/api`).<br>- Implement automated 30-second telemetry polling and validate production web build (`npm run build`). | 07/24/2026 | 07/24/2026 | Web App Testing & Build |

### Week 6 Achievements

* Successfully enabled AWS IAM Access Analyzer for passive misconfiguration detection at zero cost.
* Built a practical library of 4 forensic SQL queries in Amazon Athena enabling rapid threat hunting over S3 CloudTrail logs.
* Conducted a head-to-head empirical evaluation of custom KQL detection rules vs. GuardDuty managed findings and authored `guardduty-comparison.md`.
* Built a state-of-the-art dark-themed web dashboard UI using React and Vite under `frontend/`.
* Connected React frontend seamlessly to Python backend REST APIs with dynamic latency charts and automated 30-second telemetry polling.
