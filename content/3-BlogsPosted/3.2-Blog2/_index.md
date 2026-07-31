---
title: "Blog 2"
date: 2026-07-31
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
---

# AWS SOC DETECTION LAB — AUTOMATED THREAT DETECTION & PIPELINE OPERATIONS ON AWS

In Cloud Security, early detection and real-time response to suspicious activities are critical. The **AWS SOC Detection Lab** project is designed as an automated incident detection system combined with an **Operations Dashboard** to visualize and monitor the health of the entire pipeline.

## Core Objectives

The project focuses on addressing two main challenges:
1. **Automated Threat Detection**: Continuous monitoring of high-risk AWS operations and triggering immediate security alerts.
2. **Pipeline Observability**: Providing a dashboard interface to measure the reliability, performance, and operational health of the detection pipeline itself.

---

## High-Level Workflow

The system operates on a fully serverless Event-Driven architecture:
1. **Detection**: Sensitive operations (e.g., public S3 bucket policies, unusual IAM creation) are logged by CloudTrail and GuardDuty.
2. **Filtering**: EventBridge catches events in near real-time and immediately triggers AWS Lambda.
3. **Processing & Alerting**: Lambda classifies event severity (`CRITICAL`, `HIGH`, `MEDIUM`), dispatches email notifications via SNS, and logs execution details into DynamoDB.
4. **Visualization**: The Operations Dashboard queries the backend to display real-time system metrics and pipeline status.

---

## Key Focus of the Operations Dashboard

Unlike traditional SIEM tools that solely focus on alert contents, this **Operations Dashboard** serves as a operational telemetry aggregator (NOC/SOC Dashboard):

* **Reliability Metrics**: Tracks total executions, success rates (`SUCCESS`), and failure logs (`ERROR`).
* **Performance Benchmarking**: Measures processing latencies for Lambda execution and SNS publishing.
* **Cost Optimization**: Incorporates caching guardrails and tracks usage against AWS Free Tier thresholds.

---

## Conclusion

**AWS SOC Detection Lab** seamlessly bridges **Security**, **Automation**, and **Operations**. It provides a clear, high-level approach to securing AWS cloud infrastructure while ensuring complete visibility into the automated detection pipeline.

**🔗 Further Reading:**
- [Soc Detection Lab](https://github.com/VAENeverD1e/soc-detection-lab)
- [Operations Dashboard](https://github.com/KoangWy/ops-dashboard)