---
title: "Bản đề xuất"
date: 2026-07-30
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Hệ thống Giám sát An toàn Thông tin & Phản ứng Tự động Cloud-Native

## Phát hiện Mối đe dọa & Tự động hóa Ứng phó Sự cố An ninh Đám mây trên AWS

### 1. Tóm tắt Điều hành

Dự án này xây dựng một **Nền tảng Giám sát An toàn Thông tin & Phản ứng Tự động Cloud-Native** chuẩn doanh nghiệp trên hạ tầng đám mây AWS. Được thiết kế nhằm giải quyết triệt để khoảng trống độ trễ trong quản lý sự cố an ninh cloud, nền tảng tự động thu thập telemetry bảo mật trên hạ tầng AWS, điều hướng và xử lý các sự kiện mối đe dọa nghiêm trọng theo thời gian thực bằng công nghệ serverless (độ trễ < 10 giây), ghi log audit sự cố vào cơ sở dữ liệu vận hành, hỗ trợ truy tìm mối đe dọa bằng SQL serverless trên kho log S3, cũng như đánh giá các dịch vụ bảo mật hàng đầu của AWS (**AWS GuardDuty** & **IAM Access Analyzer**).

Hệ thống bao phủ các kịch bản tấn công cloud rủi ro cao được ánh xạ theo chuẩn **MITRE ATT&CK** (như leo thang quyền hạn IAM, tạo tài khoản backdoor, công khai S3 storage bucket và exfiltration dữ liệu hàng loạt). Tuân thủ nghiêm ngặt **Chính sách Kiểm soát Chi phí Zero-Spend ($0.00 spend)** qua AWS Free Tier và gói dùng thử có kiểm soát, toàn bộ hạ tầng đám mây được tự động hóa 100% bằng **Hạ tầng dưới dạng Mã (Terraform)**. Ngoài ra, hệ thống cung cấp giao diện tích hợp bất đồng bộ qua **Amazon SQS** phục vụ cho các Trung tâm Vận hành An toàn Thông tin (SOC) của doanh nghiệp.

### 2. Vấn đề

**Dịch chuyển lên Cloud làm thay đổi mặt trận tấn công — và kẻ tấn công khai thác sự bùng nổ lệnh API để ẩn mình.**

Khi các doanh nghiệp dịch chuyển hạ tầng, lưu trữ và định danh lên AWS, các hàng rào bảo mật truyền thống không còn hiệu quả. Tấn công môi trường cloud hiếm khi đòi hỏi các mã khai thác lỗ hổng phức tạp; thay vào đó, kẻ tấn công khai thác access key bị rò rỉ, mật khẩu console bị phished hoặc tài khoản IAM bị phân thừa quyền để thực thi các lệnh API quản trị.

Chuỗi tấn công cloud điển hình diễn ra rất nhanh chóng:
1. **Truy cập Ban đầu (Initial Access)**: Kẻ tấn công có được access key hoặc thông tin xác thực IAM.
2. **Thăm dò (Reconnaissance)**: Thực thi âm thầm các lệnh liệt kê IAM (`ListUsers`, `ListRoles`, `GetAccountAuthorizationDetails`) để thu thập sơ đồ phân quyền.
3. **Duy trì Truy cập & Leo thang Quyền (Persistence & Privilege Escalation)**: Tạo tài khoản IAM backdoor (`CreateUser`) và lập tức gán quyền quản trị tối cao (`AttachUserPolicy` với `AdministratorAccess`).
4. **Trích xuất / Công khai Dữ liệu (Exfiltration / Exposure)**: Tải dữ liệu nhạy cảm hàng loạt từ S3 bucket (`GetObject`) hoặc sửa đổi chính sách bucket (`PutBucketPolicy`) để công khai dữ liệu ra ngoài Internet (`Principal: *`).

Vì mỗi lệnh API đơn lẻ rất giống với thao tác quản trị hàng ngày, luồng thu thập log SIEM theo lô truyền thống (gây độ trễ từ 5 đến 15 phút) khiến nhóm an ninh hoàn toàn bị động. Bài toán cốt lõi là phải xây dựng một kiến trúc có khả năng **bắt chính xác các mẫu hành vi nguy hiểm tức thì (<10 giây)**, kích hoạt thông báo tự động, lưu trữ nhật ký kiểm toán bất biến và dễ dàng tích hợp với hệ thống SOC doanh nghiệp sẵn có.

#### Các bước Vận hành Chi tiết

1. **Luồng AWS Native Detect-Decide-Act**:
   - **Thu thập API & Điều hướng Sự kiện**: AWS CloudTrail ghi lại các thao tác API trên mọi region. **Amazon EventBridge** khớp các mẫu sự kiện rủi ro cao (`PutBucketPolicy`, `CreateUser` kèm gán quyền admin) và các cảnh báo học máy từ **AWS GuardDuty**.
   - **Phối hợp Quy trình bằng Step Functions**: EventBridge kích hoạt **AWS Step Functions** (`soc-detection-orchestrator`), bổ sung ngữ cảnh sự kiện và điều hướng luồng quyết định.
   - **Tự động Khắc phục Sự cố (Auto-Remediation)**: **AWS Lambda** tự động khắc phục vi phạm dưới 5 giây — ví dụ: kích hoạt lại `s3:PutPublicAccessBlock` hoặc gán chính sách `SecurityDenyAll` để cô lập ngay tài khoản IAM backdoor.
   - **Kiểm toán & Dashboard Vận hành**: Nhật ký thực thi lưu trữ vào **Amazon DynamoDB** (`automation-pipeline-events`), phát cảnh báo qua **Amazon SNS** và cập nhật tức thì lên **Ops Dashboard**.
   - **Truy tìm Mối đe dọa bằng SQL Athena**: **Amazon Athena** thực thi các truy vấn SQL chuẩn ANSI trực tiếp trên kho log CloudTrail S3 mà không cần quản lý cụm tìm kiếm.

2. **Luồng kép Ingestion & So sánh Benchmark**:
   - Log CloudTrail liên tục đẩy về S3 và được xếp hàng chờ qua **Amazon SQS** để đẩy vào **Elastic SIEM**.
   - Cả hai cơ chế phát hiện (serverless native AWS vs. luật KQL tùy biến trên Elastic) hoạt động song song, cung cấp dữ liệu thực nghiệm cho ma trận đánh giá độ trễ và độ chính xác (`detection-comparison.md`).

---

#### 4. Danh mục Dịch vụ AWS & Lý do Kỹ thuật

| Dịch vụ AWS | Vai trò Kiến trúc | Chính sách Chi phí | Lý do Kỹ thuật & Lựa chọn |
| :--- | :--- | :--- | :--- |
| **AWS CloudTrail** | Thu thập Log Audit API Đa Region | Free Tier (Management events) | Nhật ký kiểm toán native ghi nhận toàn bộ các lệnh management và data event. |
| **Amazon S3** | Kho Lưu trữ Log Trung tâm | Free Tier | Kho lưu trữ log bất biến, mã hóa SSE-S3 tại chỗ với chi phí duy trì $0 khi chờ. |
| **Amazon EventBridge** | Bộ Điều hướng Sự kiện Thời gian thực | Free Tier | Khớp mẫu JSON dưới 1 giây cho các sự kiện CloudTrail và cảnh báo GuardDuty. |
| **AWS Step Functions** | Phối hợp Quy trình Tự động (Orchestration) | Free Tier | State machine phối hợp luồng sự kiện (`Detect -> Enrich -> Decide -> Remediate`). |
| **AWS Lambda** | Xử lý & Tự động Khắc phục Sự cố | Free Tier | Thực thi Python serverless để hủy policy S3 công khai, cô lập tài khoản IAM rủi ro và ghi log DynamoDB. |
| **Amazon SNS** | Phân phối Thông báo Đa kênh | Free Tier | Gửi email cảnh báo bảo mật trực tiếp đến hộp thư quản trị viên tức thì. |
| **Amazon DynamoDB** | Cơ sở Dữ liệu Audit Telemetry | Free Tier | Cơ sở dữ liệu key-value độ trễ thấp lưu lịch sử cảnh báo với chi phí duy trì $0. |
| **Amazon Athena** | Truy vấn Log SQL Serverless | Pay-per-query ($0 khi chờ) | Truy vấn SQL chuẩn ANSI trên log S3 thô mà không cần quản lý máy chủ tìm kiếm. |
| **Amazon SQS** | Hàng chờ Đệm Tích hợp SOC | Free Tier | Hấp thụ bùng nổ log và cung cấp giao diện trích xuất bất đồng bộ cho SOC ngoài. |
| **Amazon CloudWatch** | Giám sát Hạ tầng & Cảnh báo | Free Tier | Theo dõi hoạt động S3, metric lỗi thực thi của Lambda và hàng chờ lỗi SQS DLQ. |
| **AWS GuardDuty** | Phát hiện Mối đe dọa bằng ML | 30 ngày Free Trial | Phát hiện bất thường bằng ML, so sánh đối chiếu với các quy tắc EventBridge. |
| **AWS Security Hub** | Quản lý Tuân thủ Benchmark | 30 ngày Free Trial | Quản lý tuân thủ tự động theo chuẩn CIS AWS Foundations Benchmark với Config recorder được giới hạn phạm vi. |
| **IAM Access Analyzer** | Phân tích Cấu hình Public Access | Luôn miễn phí | Phân tích chứng minh logic tĩnh phát hiện công khai S3 bucket policy trái phép. |

---

### 5. Lộ trình Triển khai Kỹ thuật

1. **Hạ tầng dưới dạng Mã (Terraform)**: Toàn bộ tài nguyên AWS được tự động hóa bằng kịch bản Terraform (`cloudtrail.tf`, `eventbridge.tf`, `lambda.tf`, `guardduty.tf`, `dynamodb.tf`, `s3.tf`, `sqs.tf`, `sns.tf`, `cloudwatch.tf`).
2. **Đường ống Serverless SOAR**: Triển khai mã Lambda Python với quyền hạn tối thiểu PoLP (`dynamodb:PutItem`, `sns:Publish`) để xử lý sự kiện JSON.
3. **Mô phỏng Tấn công & Kiểm thử**: Thực thi 5 kịch bản tấn công AWS (thăm dò IAM, tạo backdoor user, leo thang quyền, sửa S3 policy, exfiltration dữ liệu) để xác nhận cảnh báo dưới 10 giây.
4. **Truy tìm Mối đe dọa Athena SQL**: Thiết lập schema bảng DDL trên kho log S3 CloudTrail để thực thi các câu lệnh SQL threat hunting.
5. **Đánh giá So sánh GuardDuty ML**: So sánh kết quả từ GuardDuty với quy tắc EventBridge về độ trễ, khả năng tùy biến và gánh nặng bảo trì.
6. **Dashboard Vận hành Tự động**: Triển khai giao diện web (React + FastAPI + DynamoDB) theo dõi sức khỏe đường ống và độ trễ thực thi.

---

### 6. Quản lý Rủi ro & Kiểm soát Chi phí

- **Hạn mức Chi phí**: Cấu hình cảnh báo AWS Zero-Spend Budget với ngưỡng **$0.01** gửi trực tiếp về email root.
- **Quản lý Trial GuardDuty**: Đặt lịch hủy GuardDuty detector vào Ngày 25 dùng thử, đảm bảo tổng chi phí AWS duy trì **$0.00**.
- **Độ tin cậy Luồng dữ liệu**: Cấu hình SQS Dead-Letter Queue (DLQ) và cảnh báo CloudWatch Alarm theo dõi lỗi thực thi.

---

### 7. Kết quả Kỳ vọng

- **Nền tảng Bảo mật Cloud-Native Complete**: Hệ thống bảo mật hướng sự kiện trên AWS hoạt động hoàn chỉnh, cung cấp cảnh báo dưới 10 giây, phân tích SQL log và ML threat detection.
- **Triển khai IaC 100% bằng Terraform**: Khởi tạo (`terraform apply`) và dọn dẹp tài nguyên (`terraform destroy`) bằng một lệnh duy nhất.
- **Tích hợp SOC Doanh nghiệp Đa Đám mây**: Giao diện SQS tách biệt kết nối dữ liệu telemetry an toàn đám mây AWS với các hệ thống SOC/SIEM doanh nghiệp ngoài.