---
title: "Adversary Attack Simulation"
date: 2026-07-29
weight: 4
chapter: false
pre: " <b> 5.4. </b> "
---

# 5.4 Adversary Attack Simulation & AWS Threat Emulation

#### Overview

In this module, you will execute **5 end-to-end AWS Cloud attack scenarios** mapped directly to the **MITRE ATT&CK framework**, covering high-risk cloud API threats (IAM reconnaissance, backdoor user creation, privilege escalation, public S3 bucket policy tampering, and bulk cloud storage exfiltration) via both the **AWS Management Console** and **AWS CLI**.

---

#### AWS Cloud Attack Scenarios (Scenarios 1 – 5)

##### Scenario 1: AWS Root Account Console Login
- **MITRE ATT&CK**: T1078.004 (Valid Accounts: Cloud Accounts)
- **Console Execution**:
  1. Sign out of all active IAM user sessions in your browser.
  2. Navigate to [AWS Management Console Login](https://console.aws.amazon.com/).
  3. Select **Root user**, enter your root account email and password (plus MFA if enabled), and sign in.
- **CLI Execution**:
  ```bash
  # Root account logins are performed via web browser.
  # Verification via AWS CLI (returns root account identity):
  aws sts get-caller-identity
  ```
- **Telemetry Generated**: CloudTrail `ConsoleLogin` event with `aws.cloudtrail.user_identity.type: "Root"`. Triggers **Critical Severity ELK SIEM Alert** (`event.action: "ConsoleLogin" AND aws.cloudtrail.user_identity.type: "Root"`).

![Scenario 1: Root Account Console Login](/images/5-Workshop/5.4-Attack-Simulation/scenario1_s3_access.png)

---

##### Scenario 2: IAM Credential & Policy Discovery
- **MITRE ATT&CK**: T1087.004 (Account Discovery: Cloud Account), T1069.003 (Permission Groups Discovery: Cloud Groups)
- **Console Steps**:
  1. Open **AWS IAM Console**.
  2. Navigate rapidly through **Users**, **Roles**, **Policies**, and **User Groups** tabs to enumerate account permissions.
- **CLI Execution**:
  ```bash
  aws iam list-users
  aws iam list-roles
  aws iam get-account-authorization-details
  ```
- **Telemetry Generated**: Rapid burst of IAM enumeration management events (`ListUsers`, `ListRoles`, `GetAccountAuthorizationDetails`) in `us-east-1`.

![Scenario 2: IAM Discovery Console](/images/5-Workshop/5.4-Attack-Simulation/scenario2_iam_discovery.png)

---

##### Scenario 3: Persistence via Backdoor User & Privilege Escalation
- **MITRE ATT&CK**: T1136.003 (Create Account: Cloud Account), T1098 (Account Manipulation)
- **Console Steps**:
  1. Open **AWS IAM Console** → **Users** → **Add users**.
  2. Name the user `backup-svc-acct`.
  3. Under Permissions, attach `AdministratorAccess`.
- **CLI Execution**:
  ```bash
  aws iam create-user --user-name backup-svc-acct
  aws iam attach-user-policy \
    --user-name backup-svc-acct \
    --policy-arn arn:aws:iam::aws:policy/AdministratorAccess
  aws iam create-access-key --user-name backup-svc-acct
  ```
- **Telemetry Generated**: Chained `CreateUser`, `AttachUserPolicy`, and `CreateAccessKey` events for `backup-svc-acct`. Triggers Step Functions state machine to automatically attach `SecurityDenyAll` containment policy!

![Scenario 3: Creating Backdoor User Console](/images/5-Workshop/5.4-Attack-Simulation/scenario3_create_backdoor_user.png)

---

##### Scenario 4: S3 Data Exfiltration (Bulk Download)
- **MITRE ATT&CK**: T1530 (Data from Cloud Storage)
- **Prerequisite**: Opting into CloudTrail S3 Data Events (`GetObject` selector) so object-level S3 data calls are recorded in the log pipeline.
- **Console Execution**:
  1. Open **CloudTrail Console** → **Trails** → Click your trail → Edit **Data events** → Add S3 `GetObject` selector for test bucket `soclab-exfil-test-demo`.
  2. Open **Amazon S3 Console** → Select `soclab-exfil-test-demo` → Select objects → Click **Actions** → **Download as .zip** (or download multiple objects sequentially).
- **CLI Execution**:
  ```powershell
  # Setup: Create test bucket & populate 25 dummy files
  aws s3 mb s3://soclab-exfil-test-demo
  for ($i = 1; $i -le 25; $i++) {
      $file = "file$i.txt"
      "dummy file $i" | Out-File $file
      aws s3 cp $file "s3://soclab-exfil-test-demo/"
  }

  # Simulation: Execute bulk download / exfiltration
  aws s3 sync s3://soclab-exfil-test-demo/ ./exfil-test/
  ```
- **Telemetry Generated**: 25 `GetObject` CloudTrail data events recorded from a single `source.ip` within 1 second. Triggers a **SIEM Threshold Detection Rule** (`>= 20 GetObject events in 5 minutes grouped by source.ip`).

![Scenario 4: Bulk S3 Exfiltration Console](/images/5-Workshop/5.4-Attack-Simulation/scenario5_bulk_s3_download.png)

---

##### Scenario 5: S3 Bucket Made Public (Policy & BPA Modification)
- **MITRE ATT&CK**: T1537 (Transfer Data to Cloud Account / Expose Public Bucket), T1562.001 (Impair Defenses)
- **Console Execution**:
  1. Open **Amazon S3 Console** → Select `soclab-public-test-demo` → **Permissions** tab.
  2. Under **Block public access (bucket settings)**, click **Edit** → Uncheck **Block all public access** → Click **Save changes** (type `confirm`).
  3. Under **Bucket policy**, click **Edit** → Paste policy granting `*` principal `s3:GetObject` access → Click **Save changes**.
- **CLI Execution**:
  ```bash
  # Step 1: Disable Block Public Access on disposable test bucket
  aws s3api put-public-access-block \
    --bucket soclab-public-test-demo \
    --public-access-block-configuration BlockPublicAcls=false,IgnorePublicAcls=false,BlockPublicPolicy=false,RestrictPublicBuckets=false

  # Step 2: Apply public bucket policy (Principal: "*", Action: "s3:GetObject")
  aws s3api put-bucket-policy \
    --bucket soclab-public-test-demo \
    --policy file://public-policy.json
  ```
- **Telemetry Generated**: `PutPublicAccessBlock` and `PutBucketPolicy` events granting public wildcard `*` principal access. Triggers **ELK SIEM Detection Rule** within 4 minutes and AWS Step Functions state machine to automatically re-enable `s3:PutPublicAccessBlock` and remove the public policy!

![Scenario 5: Public Bucket Policy Console](/images/5-Workshop/5.4-Attack-Simulation/scenario4_public_bucket_policy.png)

---

#### AWS Telemetry Verification Checklist

- [x] Confirm AWS CloudTrail captures multi-region audit events (`us-east-1` for global IAM, `ap-southeast-2` for S3/Lambda).
- [x] Confirm S3 log bucket `soc-cloudtrail-logs-*` receives raw CloudTrail JSON logs.
- [x] Confirm SQS queue receives `ObjectCreated` event notifications for Elastic SIEM ingestion.
- [x] Confirm EventBridge rule matches `CreateUser`, `AttachUserPolicy`, `PutBucketPolicy`, and `DeleteBucketPolicy` within **< 5 seconds**.
- [x] Confirm AWS Step Functions state machine (`soc-detection-orchestrator`) executes green paths and triggers Lambda auto-remediation (reverting public S3 policies and applying `SecurityDenyAll` containment to backdoor IAM users).
- [x] Confirm AWS GuardDuty generates ML findings (`Policy:S3/BucketPublicAccessGranted`, `Exfiltration:S3/AnomalousBehavior`).
