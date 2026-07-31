---
title: "Blogs Posted"
date: 2026-07-31
weight: 3
chapter: false
pre: " <b> 3. </b> "
---

This section lists and introduces the blogs posted to [AWS Study Group](https://www.facebook.com/groups/awsstudygroupfcj).

### [Blog 1 - AMAZON EVENTBRIDGE — THE HEART OF EVENT-DRIVEN SECURITY ARCHITECTURE ON AWS](3.1-Blog1/_index.md)
This blog introduces Amazon EventBridge — AWS's serverless event bus service that serves as the central orchestrator in security detection and response automation (Security SOAR) architectures. The article analyzes how EventBridge captures CloudTrail events in near real-time, event filtering techniques using JSON patterns, Multi-Region event handling solutions, and why EventBridge's zero-polling model outperforms traditional polling approaches in security contexts.
[Image](/images/3-BlogsPosted/blog1.png)

### [Blog 2 - AWS SOC DETECTION LAB — AUTOMATED THREAT DETECTION & PIPELINE OPERATIONS ON AWS](3.2-Blog2/_index.md)
This blog presents a high-level overview of the **AWS SOC Detection Lab** project — an automated threat detection system paired with an Operations Dashboard. It briefly outlines the event-driven workflow (CloudTrail/GuardDuty to EventBridge, Lambda, SNS, and DynamoDB) and highlights the role of the dashboard in monitoring pipeline observability and health.
[Image](/images/3-BlogsPosted/blog2.png)

### [Blog 3 - TERRAFORM IaC — WHY YOU SHOULD USE INFRASTRUCTURE AS CODE FOR YOUR AWS SECURITY LAB?](3.3-Blog3/_index.md)
This blog explains why Terraform is an essential choice when building an AWS Security Lab with multiple interconnected services. The article analyzes 3 core benefits: Reproducibility (rebuild infrastructure with a single command), Security & Least Privilege (review IAM policies like regular code), and automatic Dependency Management. It also includes a practical Best Practices table and HCL code examples drawn directly from the SOC Detection Lab project.
[Image](/images/3-BlogsPosted/blog3.png)