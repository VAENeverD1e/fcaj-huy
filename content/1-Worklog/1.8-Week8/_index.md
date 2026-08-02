---
title: "Week 8 Worklog"
date: 2026-08-03
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---
### Week 8 Objectives

* Design and implement Python REST API backend service (`backend/`) using Flask/FastAPI and AWS Boto3 SDK for DynamoDB & CloudWatch.
* Build interactive frontend interface (`frontend/`) using React, Vite, and modern dark-theme CSS design system.
* Construct operational dashboard widgets: real-time pipeline status card, Free Tier cost monitor, serverless latency charts, and live security alert data table.
* Wire React frontend components to backend REST endpoints with automated telemetry polling (every 30 seconds).

### Tasks Carried Out This Week

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| Mon | - Design RESTful API architecture for Automation Ops Dashboard.<br>- Initialize Python backend project directory (`backend/`) and setup Boto3 service modules (`dynamodb_service.py`, `cloudwatch_service.py`, `cost_service.py`). | 08/03/2026 | 08/03/2026 | <https://000066.awsstudygroup.com> |
| Tue | - Construct REST API endpoints (`GET /api/alerts`, `GET /api/metrics/pipeline`, `GET /api/metrics/cost`, `GET /api/health`).<br>- Implement CORS middleware, error handlers, and sub-100ms API response caching. | 08/04/2026 | 08/04/2026 | Python API Testing |
| Wed | - Initialize React application project using Vite (`npx create-vite@latest frontend --template react`).<br>- Configure modern CSS design system, typography, dark mode palette, and responsive flex/grid layouts. | 08/05/2026 | 08/05/2026 | <https://000079.awsstudygroup.com> |
| Thu | - Build core UI components: `PipelineStatusWidget`, `CostTrackerWidget`, `LatencyChart` (line chart), `InvocationChart` (bar chart), and `AlertFeedTable` (data table with severity badges). | 08/06/2026 | 08/06/2026 | React Component Architecture |
| Fri | - Wire React frontend to Python REST API endpoints (`http://localhost:5000/api`).<br>- Add automated 30-second telemetry polling and validate production web build (`npm run build`). | 08/07/2026 | 08/07/2026 | Web App Testing & Build |

### Week 8 Achievements

* Architected and implemented a high-performance Python REST API backend service under `backend/`.
* Integrated AWS Boto3 SDK for live metric querying across DynamoDB, CloudWatch, and AWS Billing APIs.
* Built a state-of-the-art dark-themed web dashboard UI using React and Vite under `frontend/`.
* Connected React frontend seamlessly to Python backend APIs with dynamic latency charts and automated 30-second telemetry polling.
* Verified responsive layout and sub-100ms API response times across desktop and mobile viewports.
