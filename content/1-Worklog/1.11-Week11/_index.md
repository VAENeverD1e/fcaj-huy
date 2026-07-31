---
title: "Week 11 Worklog"
date: 2024-01-01
weight: 11
chapter: false
pre: " <b> 1.11. </b> "
---
### Week 11 Objectives

* Learn frontend component design, state management (`useState`, `useEffect`), REST API consumption (`fetch` / `axios`), and chart visualization libraries.
* Build the frontend interface (`frontend/`) for the **Automation Ops Dashboard**.
* Construct interactive dashboard widgets displaying real-time serverless pipeline health, invocation counts, processing latency charts, live security alert feed, and Free Tier cost monitors.

### Tasks Carried Out This Week

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| Mon | - Initialize React application project using Vite (`npx create-vite@latest frontend --template react`).<br>- Configure modern CSS design system, typography, dark mode palette, and responsive flex/grid layouts. | 08/24/2026 | 08/24/2026 | <https://000079.awsstudygroup.com> |
| Tue | - Develop Core Status Card Components:<br>&emsp;+ `PipelineStatusWidget`: Displays live health status (Healthy / Degraded / Down).<br>&emsp;+ `CostTrackerWidget`: Displays current AWS spend vs. $0 free tier budget. | 08/25/2026 | 08/25/2026 | React Component Architecture |
| Wed | - Develop Metric Charting Components:<br>&emsp;+ `LatencyChart`: Line graph rendering Lambda execution duration & SQS processing latency.<br>&emsp;+ `InvocationChart`: Bar chart showing event throughput over 24 hours. | 08/26/2026 | 08/26/2026 | Charting Library Integration |
| Thu | - Develop Live Security Alert Table Component:<br>&emsp;+ `AlertFeedTable`: Interactive data table rendering live alerts fetched from `/api/alerts`.<br>&emsp;+ Features: severity color badges, keyword search, modal popups for full CloudTrail JSON audit detail. | 08/27/2026 | 08/27/2026 | Data Table & Modal UI |
| Fri | - Wire React frontend to Flask/FastAPI backend API endpoints (`http://localhost:5000/api`).<br>- Add auto-refresh polling interval (every 30 seconds).<br>- Validate full web application build (`npm run build`). | 08/28/2026 | 08/28/2026 | Web App Testing & Build |

### Week 11 Achievements

* Built a state-of-the-art dark-themed web dashboard UI using React under `frontend/`.
* Implemented dynamic data visualization charts for serverless invocation throughput and processing latency.
* Integrated live alert feed table rendering security events directly from Amazon DynamoDB.
* Connected React frontend seamlessly to Python backend APIs with automated 30-second telemetry polling.
* Verified responsive layout and smooth UI performance across desktop and mobile viewports.
