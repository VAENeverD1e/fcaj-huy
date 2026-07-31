---
title: "Dọn dẹp Tài nguyên & Bài học"
date: 2026-07-29
weight: 6
chapter: false
pre: " <b> 5.6. </b> "
---

# 5.6 Dọn dẹp Môi trường & Bài học Kinh nghiệm

#### Tổng quan

Để duy trì nghiêm ngặt **Chính sách Kiểm soát Chi phí Zero-Spend** và tránh phát sinh chi phí cloud ngoài ý muốn sau bài workshop, hãy thực hiện quy trình dọn dẹp từng bước dưới đây bằng cả **AWS Management Console** và **Terraform CLI**. Ngoài ra, phần này ghi nhận các **Điểm Tùy biến Cá nhân** và **Bài học Kinh nghiệm Kỹ thuật** rút ra trong quá trình triển khai dự án (đáp ứng tiêu chí Thang điểm FCAJ 4.5).

---

#### 1. Quy trình Hủy bỏ Tài nguyên Từng bước

##### Bước 1: Xóa Rỗng S3 Log Bucket qua AWS Console hoặc CLI
Các bucket chứa log CloudTrail và bucket đích chứa sự kiện không thể bị xóa bởi Terraform nếu bên trong còn chứa đối tượng.
1. Mở **Amazon S3 Console**.
2. Chọn các log bucket và bucket thử nghiệm (`soc-cloudtrail-logs-*`, `soclab-exfil-test-demo`, `soclab-public-test-demo`).
3. Nhấn **Empty** và xác nhận xóa toàn bộ đối tượng dữ liệu/log.

```bash
# Các lệnh làm rỗng bucket qua CLI:
aws s3 rm s3://soc-cloudtrail-logs-demo-bucket --recursive
aws s3 rm s3://soclab-exfil-test-demo --recursive
aws s3 rm s3://soclab-public-test-demo --recursive
```

![Amazon S3 Empty Bucket Console](/images/5-Workshop/5.6-Cleanup/s3_empty_bucket_console.png)

##### Bước 2: Hủy Hạ tầng Triển khai bởi Terraform
Truy cập thư mục `terraform/` và thực thi lệnh hủy hạ tầng:
```bash
cd terraform/
terraform destroy -auto-approve
```

![Terraform Destroy Teardown Terminal Output](/images/5-Workshop/5.6-Cleanup/terraform_destroy_terminal.png)

##### Bước 3: Tắt Dịch vụ AWS GuardDuty Detector trên AWS Console
Để tránh việc tự động chuyển đổi từ dùng thử 30 ngày sang trả phí:
1. Mở **AWS GuardDuty Console** → Chọn **Settings**.
2. Cuộn xuống cuối trang và chọn **Disable GuardDuty**.
3. Xác nhận xóa detector.

![AWS GuardDuty Disable Detector Console](/images/5-Workshop/5.6-Cleanup/guardduty_disable_console.png)

##### Bước 4: Xác nhận Chi phí $0.00 trên AWS Cost Explorer
1. Mở **AWS Billing and Cost Management Console** → **Cost Explorer**.
2. Xác nhận tổng chi phí phát sinh trong tháng hiển thị **$0.00**.

![AWS Cost Explorer Zero Spend Verification Console](/images/5-Workshop/5.6-Cleanup/aws_cost_explorer_zero_spend.png)

---

#### 2. Danh mục Kiểm tra Hạn mức Chi phí (Checklist)

- [x] Xác nhận lệnh `terraform destroy` hoàn thành không còn tài nguyên cloud nào sót lại.
- [x] Xác nhận AWS Cost Explorer hiển thị tổng chi phí phát sinh **$0.00**.
- [x] Xác nhận trạng thái GuardDuty Detector là **Disabled / Deleted**.
- [x] Xác nhận các S3 Log Bucket đã được xóa hoàn toàn.
- [x] Xác nhận cảnh báo email Zero-Spend Budget vẫn đang hoạt động trên tài khoản root.

---

#### 3. Tùy biến Cá nhân & Đóng góp Kỹ thuật (Rubric 4.5)

Thay vì copy các bài lab mẫu có sẵn, dự án này đã đóng góp nhiều cải tiến kỹ thuật tùy biến mang tính sáng tạo:

1. **Kiến trúc Tách biệt Độ trễ Luồng kép (Dual-Path Latency Architecture)**:
   - Các bài lab thông thường đẩy toàn bộ log duy nhất vào SIEM. Chúng tôi đã tách biệt hệ thống thành **Đường ống Tự động hóa Serverless <10 giây** (EventBridge → Lambda → Discord/DynamoDB) và **Đường ống Tương quan SIEM ~5 phút** (S3 → SQS → Elastic Fleet). Điều này giúp các cuộc tấn công rủi ro cao (như tạo backdoor admin) lập tức phát cảnh báo mà không phải chờ ingestion theo lô.
2. **Cơ sở Dữ liệu Nhật ký Telemetry Vận hành (DynamoDB)**:
   - Triển khai bảng audit (`automation-pipeline-events`) để lưu lại mọi cảnh báo serverless đã xử lý, giúp chuyên viên kiểm tra sức khỏe đường ống và truy vết dễ dàng.
3. **Luật Tương quan Chuỗi Hành vi EQL**:
   - Xây dựng các luật EQL theo dõi trạng thái (`sequence by actor with maxspan=5m`) để phân tích chuỗi tấn công multi-step trên cloud (như `CreateUser` theo sau bởi `AttachUserPolicy`) thay vì chỉ phụ thuộc vào các trigger sự kiện đơn lẻ.

---

#### 4. Trải nghiệm Cá nhân & Những Thách thức Thực tế

Xây dựng một hệ thống phát hiện SOC hoàn chỉnh từ con số 0 là một hành trình học hỏi rất lớn. Khi bắt đầu dự án này, tôi đã gặp phải nhiều khó khăn thực tế:

- **Bỡ ngỡ giữa Hệ sinh thái AWS & Việc lựa chọn Dịch vụ**:
  - *Thách thức*: AWS có hàng trăm dịch vụ khác nhau và ban đầu tôi thực sự bối rối không biết nên chọn dịch vụ nào cho từng phần của hệ thống. Việc hiểu sự khác biệt và cách phối hợp giữa EventBridge, SQS, Step Functions và Lambda đòi hỏi tôi phải đọc tài liệu và thử nghiệm rất nhiều.
- **Nỗi lo về Chi phí & Billing Bất ngờ**:
  - *Thách thức*: Làm việc trên hạ tầng đám mây, lo lắng lớn nhất của tôi là vô tình bật nhầm dịch vụ tính phí hoặc vượt quá hạn mức Free Tier dẫn đến hóa đơn bất ngờ.
  - *Cách xử lý*: Tôi luôn kiểm tra AWS Budgets, ưu tiên các cấu hình Free Tier, thiết lập cảnh báo $0 spend và quản lý toàn bộ tài nguyên qua Terraform để có thể dọn dẹp (destroy) sạch sẽ khi hoàn thành.
- **Liên tục Gặp lỗi Cấu hình & Thử-Sai (Trial and Error)**:
  - *Thách thức*: Tôi đã vấp phải rất nhiều lỗi thực tế — từ lỗi phân quyền IAM (`AccessDenied`), cú pháp Terraform, lỗi pattern matching trong EventBridge, cho tới việc lệch region giữa IAM (`us-east-1`) và dịch vụ vùng (`ap-southeast-2`).
  - *Cách xử lý*: Tôi đã dành nhiều giờ đọc log trên CloudWatch, tra cứu tài liệu AWS và kiểm tra từng IAM policy cho đến khi dữ liệu chạy thông suốt.
- **Áp lực khi Kết nối một Pipeline Xử lý Lớn End-to-End**:
  - *Thách thức*: Việc kết nối hàng loạt thành phần — CloudTrail, SQS, EventBridge, Lambda, Step Functions, DynamoDB, Elastic SIEM và Athena — thành một pipeline hoạt động nhịp nhàng ban đầu mang lại cảm giác khá quá tải.
  - *Bài học rút ra*: Chia nhỏ bài toán thành từng bước đơn giản (thu thập log → truyền tải → tự động hóa → SIEM) đã giúp tôi kiểm soát độ phức tạp và tự tin hơn rất nhiều.

---

#### 5. Hướng dẫn Xử lý Lỗi Phổ biến & Khắc phục khi Triển khai lại

| Triệu chứng / Thông báo Lỗi | Nguyên nhân Gốc | Giải pháp & Khắc phục Chi tiết |
| :--- | :--- | :--- |
| `ResourceNotFoundException: Requested resource not found` (DynamoDB) | Đặt sai tên bảng DynamoDB (ví dụ: dùng `automation_pipeline_events` thay vì `automation-pipeline-events`). | Đảm bảo tên bảng sử dụng dấu gạch ngang (`automation-pipeline-events`) và partition key là `event_id` (Kiểu `S`). |
| CloudTrail Console Báo Lỗi: `"The lookup attribute value is not valid"` | Tìm kiếm sự kiện IAM tại region `ap-southeast-2` hoặc tìm theo `eventId` chữ thường. | Các sự kiện IAM toàn cục (`CreateUser`, `AttachUserPolicy`) phát sinh tại `us-east-1`. Sử dụng tham số `EventName=${action}` và điều hướng tới `us-east-1` cho IAM và `ap-southeast-2` cho S3. |
| `HIVE_CURSOR_ERROR: Cannot deserialize JSON` trong Athena | File JSON CloudTrail chứa bản ghi không đúng định dạng hoặc struct schema bất thường. | Đảm bảo câu lệnh DDL trong `athena.tf` khai báo `org.openx.data.jsonserde.JsonSerDe` kèm tham số `TBLPROPERTIES ("ignore.malformed.json" = "true")`. |
| Lỗi `BucketNotEmpty` khi chạy `terraform destroy` | Các file log CloudTrail vẫn còn tồn tại trong S3 log bucket. | Xóa rỗng S3 bucket trước bằng CLI (`aws s3 rm s3://[tên-bucket] --recursive`) trước khi thực thi `terraform destroy`. |
| Chi phí AWS Config Recorder vượt quá Free Tier | Config recorder được cấu hình ghi log liên tục cho tất cả loại tài nguyên trên toàn hệ thống. | Rút gọn phạm vi Config recorder trong `security_hub.tf` chỉ ghi 4 loại tài nguyên: `AWS::S3::Bucket`, `AWS::IAM::User`, `AWS::IAM::Role`, và `AWS::IAM::Policy`. |