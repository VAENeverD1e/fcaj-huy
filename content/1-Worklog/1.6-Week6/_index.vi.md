---
title: "Worklog Tuần 6"
date: 2026-07-20
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---
### Mục tiêu Tuần 6

* Tìm hiểu các dịch vụ quét an ninh thụ động IAM Access Analyzer, truy vấn log SQL serverless Amazon Athena và phát hiện đe dọa managed AWS GuardDuty.
* Bật IAM Access Analyzer để phát hiện các quyền truy cập ngoài ý muốn vào tài nguyên AWS (S3, IAM, Lambda, SQS).
* Xây dựng bộ 4 câu lệnh truy vấn SQL SOC nòng cốt trong Amazon Athena Console để săn đe dọa trên tệp log CloudTrail S3 thô.
* Bật AWS GuardDuty detector trong thời gian dùng thử 30 ngày (Free Trial) và đánh giá kết quả phát hiện so với quy tắc KQL tùy chỉnh trên SIEM.
* Viết báo cáo đánh giá so sánh chuyên sâu (`guardduty-comparison.md`) phân tích độ trễ phát hiện, phạm vi quan sát và bài toán chi phí, sau đó chủ động tắt GuardDuty trước khi hết trial.

### Các công việc triển khai trong tuần

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Bật IAM Access Analyzer với ranh giới trust zone là tài khoản AWS để đánh giá chia sẻ tài nguyên công khai/liên tài khoản.<br>- Cấu hình Athena database & workgroup trên S3 bucket lưu log CloudTrail. | 20/07/2026 | 20/07/2026 | <https://000030.awsstudygroup.com> |
| 3 | - Xây dựng và thực thi 4 câu lệnh truy vấn SQL SOC trong Athena Query Editor:<br>&emsp;+ Top địa chỉ IP gọi API thất bại<br>&emsp;+ Lịch sử tạo IAM access key<br>&emsp;+ Thay đổi chính sách S3 bucket<br>&emsp;+ Hành động API của Root user trong 7 ngày qua. | 21/07/2026 | 21/07/2026 | <https://000106.awsstudygroup.com> |
| 4 | - Bật AWS GuardDuty detector ở chế độ trial với lịch nhắc tắt dịch vụ rõ ràng.<br>- Tạo EventBridge Event Rule lọc sự kiện cảnh báo GuardDuty (`aws.guardduty`) chuyển tiếp tới hàm Lambda. | 22/07/2026 | 22/07/2026 | <https://000098.awsstudygroup.com> |
| 5 | - Tái thực thi Kịch bản Tấn công Cloud 8–12 (Đăng nhập Root không MFA, dò quét IAM, tạo persistence key, rút dữ liệu S3).<br>- Ghi nhận dữ liệu thực nghiệm: Độ trễ KQL tùy chỉnh (~4–13 phút) so với giới hạn baseline và thời gian phát hiện của GuardDuty. | 23/07/2026 | 23/07/2026 | Empirical Testing Data |
| 6 | - Hoàn thiện báo cáo đánh giá so sánh chuyên sâu (`guardduty-comparison.md`).<br>- Chủ động tắt GuardDuty detector và S3 Protection trước khi hết hạn trial để giữ nguyên chi phí AWS ở mức $0. | 24/07/2026 | 24/07/2026 | <https://000098.awsstudygroup.com> |

### Kết quả đạt được Tuần 6

* Bật thành công AWS IAM Access Analyzer tự động phát hiện cấu hình sai thụ động ở mức chi phí $0.
* Xây dựng bộ 4 câu lệnh truy vấn SQL điều tra sự cố thực tế trong Amazon Athena phục vụ săn đe dọa trên tệp log CloudTrail S3 thô.
* Tích hợp dịch vụ phát hiện đe dọa AWS GuardDuty vào luồng cảnh báo serverless qua EventBridge.
* Thực hiện so sánh đối chứng thực nghiệm giữa luật KQL tùy chỉnh và cảnh báo GuardDuty, hoàn thiện báo cáo `guardduty-comparison.md`.
* Đảm bảo kỷ luật chi phí nghiêm ngặt khi tắt GuardDuty trước khi hết trial, duy trì tổng chi phí AWS ở mức $0.
