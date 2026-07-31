---
title: "Blog 1"
date: 2026-07-31
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

# AMAZON EVENTBRIDGE — THE HEART OF EVENT-DRIVEN SECURITY ARCHITECTURE ON AWS

Amazon EventBridge is a **serverless event bus** service fully managed by AWS, enabling you to connect applications with each other through real-time events. In Security Detection & Response systems, EventBridge acts as the "heart" that orchestrates the entire automation pipeline.

## How Does EventBridge Work?

When a user performs an action on AWS (e.g., creating a new IAM User, modifying an S3 Bucket policy), CloudTrail records that event. EventBridge continuously listens to these events and matches them against pre-defined **Event Rules**. If matched, EventBridge immediately triggers a Target — which can be a Lambda Function, SQS Queue, SNS Topic, and more.

```
CloudTrail API Call
        │
        ▼
EventBridge Event Bus
        │ (Pattern Matching)
        ▼
Event Rule Match
        │
        ▼
Lambda Function (Target)
```

## Why Is EventBridge Ideal for Security Automation?

**1. Near Real-time Detection**
The latency from when an event occurs to when Lambda is triggered is typically just a few seconds — far faster than periodically polling CloudTrail logs from S3.

**2. Event Pattern Filtering**
You can precisely define which events to capture using JSON pattern syntax. For example, only catching API Calls with `eventName` equal to `CreateUser` or `AttachUserPolicy`:

```json
{
  "source": ["aws.iam"],
  "detail-type": ["AWS API Call via CloudTrail"],
  "detail": {
    "eventName": ["CreateUser", "AttachUserPolicy", "CreateAccessKey"]
  }
}
```

**3. Multi-Region Support**
One particularly important point: AWS emits global IAM and Console Sign-in events in the `us-east-1` region, regardless of where your actual resources reside. The solution is to create a separate Event Rule in `us-east-1` pointing to a Lambda Function in your primary region (e.g., `ap-southeast-2`).

**4. Zero-Polling Architecture**
Instead of scheduling periodic log checks (which consume DynamoDB RCU and Lambda invocations), EventBridge only triggers Lambda when events actually occur — saving significant costs.

## Key Points to Remember

* A single Event Rule can have multiple **Targets** (up to 5), e.g., calling Lambda while simultaneously pushing to SQS.
* Enable a **Dead Letter Queue (DLQ)** for the Lambda Target to avoid losing events when Lambda fails.
* EventBridge guarantees **at-least-once delivery** — Lambda may be invoked more than once for the same event, so ensure your handler is **idempotent**.
* **Custom Event Pricing**: AWS charges **$1.00/million events** from the very first event — **there is no Free Tier for custom events**. However, **AWS Management Events** (including CloudTrail API calls) are ingested into the default event bus **completely free of charge** — this is exactly what our project uses. Note: EventBridge Scheduler has a separate free tier of 14 million invocations/month (a different feature).

With EventBridge, you can build a completely serverless AWS threat detection system that responds instantly and requires no server management — this is the foundation of the **AWS-Native SOAR (Security Orchestration, Automation and Response)** model.

**🔗 Further Reading:**
- [Amazon EventBridge Documentation](https://docs.aws.amazon.com/eventbridge/latest/userguide/what-is-amazon-eventbridge.html)
- [EventBridge Event Patterns](https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-event-patterns.html)