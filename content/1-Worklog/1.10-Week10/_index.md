---
title: "Week 10 Worklog"
date: 2024-01-01
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---
### Week 10 Objectives

* Understand Python web backend development concepts: RESTful API design, HTTP status codes, CORS middleware, and environment configurations.
* Learn AWS Boto3 SDK query integration for Amazon DynamoDB and Amazon CloudWatch metric retrieval.
* Build the backend service (`backend/`) for the **Automation Ops Dashboard** using Python (Flask / FastAPI).
* Construct REST API endpoints delivering real-time pipeline status, alert history, serverless invocation counts, processing latency, and AWS Free Tier billing metrics.

### Tasks Carried Out This Week

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| Mon | - Design RESTful API architecture for the Automation Ops Dashboard.<br>- Set up Python project directory (`backend/`), configure virtual environment (`venv`), and install dependencies (`Flask`/`FastAPI`, `boto3`, `flask-cors`, `python-dotenv`). | 08/17/2026 | 08/17/2026 | <https://000066.awsstudygroup.com> |
| Tue | - Develop Boto3 DynamoDB integration module (`backend/services/dynamodb_service.py`).<br>- Implement API endpoint `GET /api/alerts`: queries the `SecurityAlerts` table with pagination, filtering by severity and timestamp range. | 08/18/2026 | 08/18/2026 | <https://000060.awsstudygroup.com> |
| Wed | - Develop Boto3 CloudWatch metric integration module (`backend/services/cloudwatch_service.py`).<br>- Implement API endpoint `GET /api/metrics/pipeline`: fetches Lambda invocation counts, execution durations, SQS queue depth, and error rates. | 08/19/2026 | 08/19/2026 | <https://000008.awsstudygroup.com> |
| Thu | - Develop Free Tier cost tracking module (`backend/services/cost_service.py`).<br>- Implement API endpoint `GET /api/metrics/cost`: queries AWS Budgets / CloudWatch estimated charges to report current spend vs. $0 free tier guardrails. | 08/20/2026 | 08/20/2026 | <https://000007.awsstudygroup.com> |
| Fri | - Implement CORS middleware, error handling handlers, and API response caching.<br>- Write unit tests (`pytest`) for API endpoints.<br>- Validate local backend API server (`http://localhost:5000/api/health`). | 08/21/2026 | 08/21/2026 | Python API Testing |

### Week 10 Achievements

* Architected and implemented a high-performance Python REST API backend service under `backend/`.
* Integrated AWS Boto3 SDK for live metric querying across DynamoDB, CloudWatch, and AWS Billing APIs.
* Created 4 core operational endpoints: `/api/alerts`, `/api/metrics/pipeline`, `/api/metrics/cost`, and `/api/health`.
* Built robust error handling and CORS support for seamless integration with modern web frontends.
* Validated backend API responsiveness with sub-100ms response times for telemetry aggregations.
