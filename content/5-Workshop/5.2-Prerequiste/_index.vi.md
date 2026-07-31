---
title: "Tiền đề & Chuẩn bị"
date: 2026-07-29
weight: 2
chapter: false
pre: " <b> 5.2. </b> "
---

# 5.2 Tiền đề & Chuẩn bị Môi trường

#### Tổng quan Yêu cầu

Trước khi triển khai SOC Detection Lab, bạn cần chuẩn bị và xác nhận cấu hình cho **Tài khoản AWS Management Console**, **Quyền hạn triển khai IAM**, **Biện pháp kiểm soát chi phí AWS Zero-Spend Budget**, **Elastic SIEM (Elastic Cloud hoặc ELK Stack trên VM)**, và **Môi trường máy ảo nạn nhân phục vụ giả lập tấn công**.

---

#### 1. Cấu hình Tài khoản AWS & Quyền IAM

##### Bước 1.1: Đăng nhập vào AWS Management Console
Truy cập [AWS Management Console](https://console.aws.amazon.com/) và đăng nhập bằng tài khoản root hoặc tài khoản IAM có quyền quản trị.

![AWS Management Console Login](/images/5-Workshop/5.2-Prerequisite/aws_console_login.png)

##### Bước 1.2: Cấu hình Tài khoản IAM & Access Key
1. Mở **IAM Console** bằng cách tìm kiếm **IAM** trên thanh tìm kiếm phía trên.
2. Chọn **Users** → **Add users** (ví dụ: `soc-lab-admin`).
3. Tại mục **Permissions options**, chọn **Attach policies directly** và gán chính sách `AdministratorAccess` (hoặc policy tối thiểu tùy chỉnh).
4. Chuyển sang tab **Security credentials** và nhấn **Create access key**. Chọn mục **Command Line Interface (CLI)**.
5. Lưu lại **Access Key ID** và **Secret Access Key** ở vị trí an toàn.

![AWS IAM User & Security Credentials Console](/images/5-Workshop/5.2-Prerequisite/aws_iam_console.png)

##### Bước 1.3: Cấu hình AWS CLI (trên FLARE-VM / Terminal Thực hành)
Mở terminal (bên trong máy ảo **FLARE-VM** hoặc máy trạm của bạn) và xác thực môi trường AWS:
```bash
aws configure
# AWS Access Key ID [None]: <NHAP_AWS_ACCESS_KEY_ID_CUA_BAN>
# AWS Secret Access Key [None]: <NHAP_AWS_SECRET_ACCESS_KEY_CUA_BAN>
# Default region name [None]: us-east-1
# Default output format [None]: json

# Kiểm tra xác thực thành công:
aws sts get-caller-identity
```

![AWS CLI Configuration Verification](/images/5-Workshop/5.2-Prerequisite/aws_cli_configure.png)

---

#### 2. Cấu hình AWS Zero-Spend Budget (Kiểm soát Chi phí)

Để đảm bảo tuyệt đối không phát sinh chi phí trong suốt quá trình thực hành:

##### Cách 1: Qua AWS Console:
1. Mở **AWS Billing and Cost Management Console**.
2. Chọn **Budgets** ở menu bên trái và nhấn **Create budget**.
3. Chọn mẫu **Zero spend budget**.
4. Nhập email nhận thông báo cảnh báo nếu chi phí phát sinh vượt quá **$0.01**.

##### Cách 2: Qua AWS CLI:
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
    "Subscribers": [{ "SubscriptionType": "EMAIL", "Address": "email-cua-ban@example.com" }]
  }]'
```

![AWS Billing & Zero Spend Budget Console](/images/5-Workshop/5.2-Prerequisite/aws_free_tier_budget.png)

---

#### 3. Cấu hình Elastic SIEM & Môi trường Thử nghiệm

##### 3.1 Môi trường Elastic SIEM (Cloud SaaS hoặc ELK Stack trên VM)
Bạn có thể triển khai Elastic SIEM linh hoạt theo 2 hình thức:
- **Tùy chọn A — Elastic Cloud (SaaS)**: Đăng ký tài khoản dùng thử 14 ngày miễn phí tại [elastic.co](https://cloud.elastic.co/) và khởi tạo deployment tên `SOC-Detection-Lab`.
- **Tùy chọn B — ELK Stack trên VM (Tự quản lý)**: Triển khai Elasticsearch + Kibana + Fleet Server trên máy ảo của riêng bạn (VirtualBox, VMware, Proxmox, AWS EC2, v.v.).

Dù sử dụng mô hình nào, bạn chỉ cần đảm bảo có đủ các thông số sau:
1. **Kibana Console URL** (Đường dẫn Cloud hoặc `http://<VM_IP>:5601`).
2. **Elasticsearch Endpoint & API Key / Quyền xác thực**.
3. **Fleet Server & Integrations**: Cài đặt integration **AWS CloudTrail** trong Kibana (**Fleet** → **Integrations**) để kéo log từ hàng chờ SQS.

![Elastic Kibana Console Homepage & Fleet Integrations](/images/5-Workshop/5.2-Prerequisite/elastic_kibana_home.png)

##### 3.2 Máy trạm Nạn nhân (Mục tiêu Giả lập Tấn công)
- **Vai trò**: Dùng duy nhất làm môi trường mục tiêu để thực thi các kịch bản giả lập tấn công (như trích xuất credential, nâng quyền, hoặc chạy kịch bản dò quét API). Bộ thu thập log trên VM sẽ ghi nhận các hành vi này và đẩy về SIEM.
- **Nền tảng**: Máy ảo VM hoặc môi trường lab riêng biệt (như Windows cài Sysmon / FLARE-VM hoặc Linux VM).
- *Lưu ý*: Việc triển khai hạ tầng bằng **Terraform** và các lệnh quản trị **AWS CLI** được thực thi trực tiếp trên **máy quản trị cục bộ (máy host của bạn)**, không chạy bên trong máy ảo nạn nhân.

---

#### 4. Danh mục Công cụ & Lệnh Kiểm tra Phiên bản

Trước khi chuyển sang bước triển khai, hãy đảm bảo môi trường thực hành của bạn đã được cài đặt và kiểm tra thành công các công cụ sau:

| Công cụ | Phiên bản Tối thiểu | Lệnh Nhanh Kiểm tra Phiên bản |
| :--- | :--- | :--- |
| **AWS CLI** | v2.x | `aws --version` |
| **HashiCorp Terraform** | v1.5.0+ | `terraform -version` |
| **Python** | 3.11+ | `python3 --version` |
| **Node.js / npm** | v18+ / v9+ | `node --version` |