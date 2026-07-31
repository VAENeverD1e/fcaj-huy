---
title: "Mô phỏng Tấn công Thực tế"
date: 2026-07-29
weight: 4
chapter: false
pre: " <b> 5.4. </b> "
---

# 5.4 Mô phỏng Tấn công & Thử nghiệm Mối đe dọa Cloud AWS

#### Tổng quan

Trong bài lab này, bạn sẽ thực thi **5 kịch bản tấn công đám mây AWS end-to-end** được ánh xạ trực tiếp theo chuẩn **MITRE ATT&CK**, bao phủ các mối đe dọa API nguy cơ cao (thăm dò phân quyền IAM, tạo tài khoản backdoor, leo thang quyền admin, sửa đổi chính sách S3 công khai trái phép và exfiltration dữ liệu cloud) thông qua cả **AWS Management Console** và **AWS CLI**.

---

#### Các Kịch bản Tấn công Đám mây AWS (Scenarios 1 – 5)

##### Kịch bản 1: Đăng nhập Root Account trên AWS Console
- **MITRE ATT&CK**: T1078.004 (Valid Accounts: Cloud Accounts)
- **Thực thi trên Console**:
  1. Đăng xuất khỏi tất cả phiên làm việc người dùng IAM trên trình duyệt.
  2. Truy cập [AWS Management Console Login](https://console.aws.amazon.com/).
  3. Chọn **Root user**, nhập email quản trị root và mật khẩu (kèm MFA nếu có), sau đó đăng nhập.
- **Thực thi qua CLI**:
  ```bash
  # Đăng nhập root account được thực hiện qua trình duyệt web.
  # Kiểm tra xác thực qua AWS CLI (trả về root account identity):
  aws sts get-caller-identity
  ```
- **Telemetry Phát sinh**: Sự kiện CloudTrail `ConsoleLogin` với `aws.cloudtrail.user_identity.type: "Root"`. Kích hoạt **Cảnh báo ELK SIEM Mức độ Rất cao (Critical)** (`event.action: "ConsoleLogin" AND aws.cloudtrail.user_identity.type: "Root"`).

![Kịch bản 1: Đăng nhập Root Account Console](/images/5-Workshop/5.4-Attack-Simulation/scenario1_s3_access.png)

---

##### Kịch bản 2: Thăm dò Thông tin Xác thực & Chính sách IAM
- **MITRE ATT&CK**: T1087.004 (Account Discovery: Cloud Account), T1069.003 (Permission Groups Discovery: Cloud Groups)
- **Các bước trên Console**:
  1. Mở **AWS IAM Console**.
  2. Chuyển đổi nhanh qua các tab **Users**, **Roles**, **Policies**, và **User Groups** để liệt kê danh mục phân quyền.
- **Thực thi qua CLI**:
  ```bash
  aws iam list-users
  aws iam list-roles
  aws iam get-account-authorization-details
  ```
- **Telemetry Phát sinh**: Bùng nổ các lệnh liệt kê IAM management events (`ListUsers`, `ListRoles`, `GetAccountAuthorizationDetails`) tại `us-east-1`.

![Kịch bản 2: Thăm dò IAM Console](/images/5-Workshop/5.4-Attack-Simulation/scenario2_iam_discovery.png)

---

##### Kịch bản 3: Duy trì Truy cập qua Backdoor User & Nâng quyền
- **MITRE ATT&CK**: T1136.003 (Create Account: Cloud Account), T1098 (Account Manipulation)
- **Các bước trên Console**:
  1. Mở **AWS IAM Console** → **Users** → **Add users**.
  2. Đặt tên user `backup-svc-acct`.
  3. Tại mục Permissions, gán chính sách `AdministratorAccess`.
- **Thực thi qua CLI**:
  ```bash
  aws iam create-user --user-name backup-svc-acct
  aws iam attach-user-policy \
    --user-name backup-svc-acct \
    --policy-arn arn:aws:iam::aws:policy/AdministratorAccess
  aws iam create-access-key --user-name backup-svc-acct
  ```
- **Telemetry Phát sinh**: Chuỗi sự kiện `CreateUser`, `AttachUserPolicy`, và `CreateAccessKey` khởi tạo cho user `backup-svc-acct`. Kích hoạt Step Functions state machine tự động gán chính sách cô lập `SecurityDenyAll`!

![Kịch bản 3: Tạo Backdoor User Console](/images/5-Workshop/5.4-Attack-Simulation/scenario3_create_backdoor_user.png)

---

##### Kịch bản 4: Trích xuất Dữ liệu S3 Hàng loạt (Bulk Download)
- **MITRE ATT&CK**: T1530 (Data from Cloud Storage)
- **Điều kiện tiên quyết**: Bật CloudTrail S3 Data Events (bộ lọc `GetObject`) để các truy vấn cấp đối tượng dữ liệu được ghi nhận vào luồng log.
- **Thực thi trên Console**:
  1. Mở **CloudTrail Console** → Chọn **Trails** → Nhấp trail của bạn → Chỉnh sửa **Data events** → Thêm bộ lọc S3 `GetObject` cho bucket `soclab-exfil-test-demo`.
  2. Mở **Amazon S3 Console** → Chọn `soclab-exfil-test-demo` → Chọn hàng loạt file → Chọn **Actions** → **Download as .zip** (hoặc tải liên tiếp nhiều file).
- **Thực thi qua CLI**:
  ```powershell
  # Khởi tạo: Tạo bucket thử nghiệm & tải lên 25 file dữ liệu mẫu
  aws s3 mb s3://soclab-exfil-test-demo
  for ($i = 1; $i -le 25; $i++) {
      $file = "file$i.txt"
      "dummy file $i" | Out-File $file
      aws s3 cp $file "s3://soclab-exfil-test-demo/"
  }

  # Thử nghiệm: Thực thi tải về hàng loạt (Exfiltration)
  aws s3 sync s3://soclab-exfil-test-demo/ ./exfil-test/
  ```
- **Telemetry Phát sinh**: 25 sự kiện `GetObject` được ghi nhận từ duy nhất 1 địa chỉ `source.ip` trong khoảng thời gian xấp xỉ 1 giây. Kích hoạt **Luật phát hiện Threshold trên SIEM** (`>= 20 sự kiện GetObject trong 5 phút gom nhóm theo source.ip`).

![Kịch bản 4: Trích xuất Dữ liệu S3 Hàng loạt Console](/images/5-Workshop/5.4-Attack-Simulation/scenario5_bulk_s3_download.png)

---

##### Kịch bản 5: Công khai S3 Bucket (Chính sách Bucket & Cấu hình BPA)
- **MITRE ATT&CK**: T1537 (Transfer Data to Cloud Account / Expose Public Bucket), T1562.001 (Impair Defenses)
- **Thực thi trên Console**:
  1. Mở **Amazon S3 Console** → Chọn `soclab-public-test-demo` → Chọn tab **Permissions**.
  2. Tại mục **Block public access (bucket settings)**, nhấn **Edit** → Bỏ chọn **Block all public access** → Nhấn Save changes (gõ `confirm`).
  3. Tại mục **Bucket policy**, nhấn **Edit** → Dán chính sách cấp quyền `s3:GetObject` cho đại diện `*` → Nhấn Save changes.
- **Thực thi qua CLI**:
  ```bash
  # Bước 1: Vô hiệu hóa Block Public Access trên bucket thử nghiệm
  aws s3api put-public-access-block \
    --bucket soclab-public-test-demo \
    --public-access-block-configuration BlockPublicAcls=false,IgnorePublicAcls=false,BlockPublicPolicy=false,RestrictPublicBuckets=false

  # Bước 2: Áp dụng chính sách bucket công khai (Principal: "*", Action: "s3:GetObject")
  aws s3api put-bucket-policy \
    --bucket soclab-public-test-demo \
    --policy file://public-policy.json
  ```
- **Telemetry Phát sinh**: Các sự kiện `PutPublicAccessBlock` và `PutBucketPolicy` cấp quyền truy cập cho đại diện `*`. Kích hoạt **Luật phát hiện KQL trên Elastic SIEM** trong ~4 phút và Step Functions state machine tự động bật lại `s3:PutPublicAccessBlock` đồng thời gỡ bỏ policy công khai!

![Kịch bản 5: Công khai S3 Bucket Policy Console](/images/5-Workshop/5.4-Attack-Simulation/scenario4_public_bucket_policy.png)

---

#### Danh mục Kiểm tra Telemetry Đám mây AWS

- [x] Xác nhận AWS CloudTrail ghi nhận đầy đủ log audit đa region (`us-east-1` cho IAM toàn cục, `ap-southeast-2` cho S3/Lambda).
- [x] Xác nhận S3 log bucket `soc-cloudtrail-logs-*` nhận file log CloudTrail JSON thô.
- [x] Xác nhận SQS queue nhận thông báo sự kiện `ObjectCreated` phục vụ thu thập log SIEM.
- [x] Xác nhận quy tắc EventBridge khớp các sự kiện `CreateUser`, `AttachUserPolicy`, `PutBucketPolicy`, và `DeleteBucketPolicy` dưới **< 5 giây**.
- [x] Xác nhận AWS Step Functions state machine (`soc-detection-orchestrator`) thực thi luồng màu xanh và kích hoạt Lambda auto-remediation (hoàn tác S3 policy và gán chính sách `SecurityDenyAll` cô lập backdoor user IAM).
- [x] Xác nhận AWS GuardDuty phát cảnh báo học máy ML (`Policy:S3/BucketPublicAccessGranted`, `Exfiltration:S3/AnomalousBehavior`).
