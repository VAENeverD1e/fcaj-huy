---
title: "Prerequisites"
date: 2026-07-29
weight: 2
chapter: false
pre: " <b> 5.2. </b> "
---

# 5.2 Prerequisites & Environment Setup

#### Requirements Overview

Before deploying the SOC Detection Lab, verify that you have configured your **AWS Management Console accounts**, **IAM deployment credentials**, **AWS Zero-Spend Budget guardrails**, **Elastic SIEM (Elastic Cloud or VM-hosted ELK)**, and **victim attack simulation environment**.

---

#### 1. AWS Account & IAM Console Setup

##### Step 1.1: Log into the AWS Management Console

Navigate to [AWS Management Console](https://console.aws.amazon.com/) and log in with your root or administrator IAM account.

![AWS Management Console Login](/images/5-Workshop/5.2-Prerequisite/aws_console_login.png)

##### Step 1.2: Configure IAM User Credentials

1. Open the **IAM Console** by searching for **IAM** in the top search bar.
2. Select **Users** → **Add users** (e.g., `soc-lab-admin`).
3. Under **Permissions options**, select **Attach policies directly** and attach `AdministratorAccess` (or custom minimal policy).
4. Navigate to the **Security credentials** tab and click **Create access key**. Select **Command Line Interface (CLI)**.
5. Save your **Access Key ID** and **Secret Access Key** securely.

![AWS IAM User & Security Credentials Console](/images/5-Workshop/5.2-Prerequisite/aws_iam_console.png)

##### Step 1.3: Configure AWS CLI (on FLARE-VM / Workstation Terminal)

Open your terminal (inside your **FLARE-VM** or local terminal) and authenticate your AWS environment:

```bash
aws configure
# AWS Access Key ID [None]: <YOUR_AWS_ACCESS_KEY_ID>
# AWS Secret Access Key [None]: <YOUR_AWS_SECRET_ACCESS_KEY>
# Default region name [None]: us-east-1
# Default output format [None]: json

# Verify authentication:
aws sts get-caller-identity
```

![AWS CLI Configuration Verification](/images/5-Workshop/5.2-Prerequisite/aws_cli_configure.png)

---

#### 2. AWS Zero-Spend Budget Setup (Cost Guardrail)

To ensure zero financial impact during your workshop execution:

##### Console Method

1. Open the **AWS Billing and Cost Management Console**.
2. Select **Budgets** from the left navigation pane and click **Create budget**.
3. Choose **Zero spend budget** as the template.
4. Set your notification email to receive alerts if spend exceeds **$0.01**.

##### AWS CLI Method

```bash
aws budgets create-budget \
  --account-id $(aws sts get-caller-identity --query "Account" --output text) \
  --budget '{
    "BudgetName": "SOC-Lab-Zero-Spend-Guardrail",
    "BudgetLimit": { "Amount": "0.01", "Unit": "USD" },
    "CostFilters": {},
    "CostTypes": { "IncludeTax": true, "IncludeSubscription": true, "UseBlended": false },
    "TimeUnit": "MONTHLY",
    "BudgetType": "COST"
  }' \
  --notifications-with-subscribers '[{
    "Notification": {
      "NotificationType": "ACTUAL",
      "ComparisonOperator": "GREATER_THAN",
      "Threshold": 100,
      "ThresholdType": "PERCENTAGE"
    },
    "Subscribers": [{ "SubscriptionType": "EMAIL", "Address": "your-email@example.com" }]
  }]'
```

![AWS Billing & Zero Spend Budget Console](/images/5-Workshop/5.2-Prerequisite/aws_free_tier_budget.png)

---

#### 3. Elastic SIEM & Victim Workstation Setup

##### 3.1 Elastic SIEM Environment (Cloud SaaS or VM-Hosted ELK)

You can deploy Elastic SIEM using either **Elastic Cloud** or a **Self-Hosted VM (ELK Stack)**:

- **Option A — Elastic Cloud (SaaS)**: Sign up for a 14-day free trial at [elastic.co](https://cloud.elastic.co/) and create a deployment named `SOC-Detection-Lab`.
- **Option B — VM-Hosted ELK (Self-Hosted)**: Deploy Elasticsearch + Kibana + Fleet Server inside your own Virtual Machine (VirtualBox, VMware, Proxmox, AWS EC2, etc.).

Regardless of deployment model, ensure you have configured:

1. **Kibana Console URL** (Cloud URL or `http://<VM_IP>:5601`).
2. **Elasticsearch Endpoint & API Key / Service Credentials**.
3. **Fleet Server & Integrations**: Install the **AWS CloudTrail** integration in Kibana (**Fleet** → **Integrations**) to enable SQS log polling.

![Elastic Kibana Console Homepage & Fleet Integrations](/images/5-Workshop/5.2-Prerequisite/elastic_kibana_home.png)

##### 3.2 Victim Workstation (Attack Simulation Target)

- **Role**: Dedicated strictly for executing attack simulations (e.g., credential dumping, privilege escalation, or cloud API reconnaissance scripts). Local log agents capture these events and forward them to your SIEM.
- **Platform**: A Virtual Machine or isolated host (e.g., Windows with Sysmon / FLARE-VM or Linux VM).
![Victim Windows](/images/5-Workshop/5.2-Prerequisite/victim-windows.png)

---

#### 4. Required Tools & Version Verification

Before proceeding to deployment, ensure your management environment has the following tools installed and verified:

| Tool | Minimum Version | Quick Verification Command |
| :--- | :--- | :--- |
| **AWS CLI** | v2.x | `aws --version` |
| **HashiCorp Terraform** | v1.5.0+ | `terraform -version` |
| **Python** | 3.11+ | `python3 --version` |
| **Node.js / npm** | v18+ / v9+ | `node --version` |
