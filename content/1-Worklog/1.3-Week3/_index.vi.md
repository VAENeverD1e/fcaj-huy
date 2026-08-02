---
title: "Worklog Tuần 3"
date: 2026-06-29
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---
### Mục tiêu Tuần 3

* Học kiến thức lưu trữ Amazon S3: Tạo bucket, vòng đời dữ liệu (lifecycle), mã hóa server-side (SSE-S3), bucket policy và quyền truy cập (ACL).
* Nắm vững các khái niệm IAM nâng cao: IAM Role, Trust Policy, Service Role, nguyên tắc quyền tối thiểu (Least Privilege) và API AssumeRole.
* Hiểu kiến trúc ghi nhật ký AWS CloudTrail: Management Events vs. Data Events, cấu hình trail đa region và xác minh tính toàn vẹn tệp log.
* Xây dựng luồng đẩy nhật ký hạ tầng Cloud: AWS CloudTrail → Amazon S3 → Amazon SQS → Elastic Agent AWS Integration Fleet.

### Các công việc triển khai trong tuần

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Tìm hiểu kiến thức lưu trữ Amazon S3 & kiểm soát bảo mật.<br>- Tạo S3 bucket an toàn (`soc-cloudtrail-logs-lab`) với mã hóa SSE-S3, bật Block Public Access và chính sách bắt buộc mã hóa HTTPS/TLS. | 29/06/2026 | 29/06/2026 | <https://000057.awsstudygroup.com><br><https://000069.awsstudygroup.com> |
| 3 | - Nghiên cứu IAM Role & Trust Relationship.<br>- Xây dựng các chính sách IAM phân quyền tối thiểu và service role (`ElasticAgentCloudTrailRole`) phục vụ thu thập log an toàn không cần chứng thư dài hạn. | 30/06/2026 | 30/06/2026 | <https://000002.awsstudygroup.com><br><https://000048.awsstudygroup.com> |
| 4 | - Bật AWS CloudTrail trên tất cả region đẩy dữ liệu về S3 bucket.<br>- Cấu hình S3 Event Notification để gửi thông báo sự kiện tạo tệp (ObjectCreated) tới Amazon SQS queue (`soc-cloudtrail-sqs-queue`). | 01/07/2026 | 01/07/2026 | <https://cloudjourney.awsstudygroup.com/3-optimize/> |
| 5 | - Cấu hình SQS queue policy chỉ chấp nhận thông báo sự kiện từ S3 bucket chỉ định.<br>- Cấu hình Dead Letter Queue (DLQ) để xử lý các message bị lỗi. | 02/07/2026 | 02/07/2026 | <https://000077.awsstudygroup.com> |
| 6 | - Cấu hình tích hợp AWS Integration trên Elastic Agent Fleet (`aws-cloudtrail` data stream).<br>- Kết nối Elastic SIEM tới endpoint SQS queue.<br>- Xác minh luồng log CloudTrail đẩy thành công về Elastic SIEM (`logs-aws.cloudtrail-*`). | 03/07/2026 | 03/07/2026 | Elastic AWS Integration Docs |

### Kết quả đạt được Tuần 3

* Nắm vững kỹ thuật gia cố bảo mật Amazon S3 và phân quyền truy cập dựa trên IAM Role.
* Triển khai thành công kiến trúc thu thập log audit đa region AWS CloudTrail về kho lưu trữ mã hóa S3.
* Thiết kế đường ống chuyển tiếp log bất đồng bộ dựa trên sự kiện sử dụng S3 Event Notification và Amazon SQS.
* Đẩy thành công dữ liệu CloudTrail về Elastic SIEM để phục vụ giám sát an ninh tập trung.
* Đảm bảo tối ưu chi phí, hoàn toàn nằm trong hạn mức AWS Free Tier.
