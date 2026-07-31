---
title: "Worklog Tuần 8"
date: 2024-01-01
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---
### Mục tiêu Tuần 8

* Hiểu các khái niệm phát hiện đe dọa managed của AWS GuardDuty: Nguồn tri thức đe dọa (threat intelligence), phát hiện bất thường bằng máy học, S3 Protection và các loại cảnh báo GuardDuty.
* Bật GuardDuty detector trong thời gian dùng thử 30 ngày (Free Trial) với lịch nhắc nhở tắt tự động để đảm bảo không phát sinh chi phí.
* Tích hợp cảnh báo GuardDuty với EventBridge để chuyển tiếp JSON payload cảnh báo vào luồng tự động hóa serverless.
* Chạy lại 5 kịch bản tấn công Cloud để đánh giá kết quả phát hiện của GuardDuty so với các quy tắc KQL tùy chỉnh trên SIEM.
* Viết báo cáo đánh giá so sánh chuyên sâu (`guardduty-comparison.md`) phân tích độ trễ phát hiện, phạm vi quan sát và bài toán chi phí.

### Các công việc triển khai trong tuần

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Nghiên cứu kiến trúc AWS GuardDuty, các dạng cảnh báo (`Recon:IAMUser`, `Persistence:IAMUser`, `Exfiltration:S3`) và cơ chế S3 Protection.<br>- Bật GuardDuty detector ở chế độ trial với lịch ghi nhớ tắt dịch vụ rõ ràng. | 03/08/2026 | 03/08/2026 | <https://000098.awsstudygroup.com> |
| 3 | - Tạo EventBridge Event Rule lọc sự kiện cảnh báo GuardDuty (`aws.guardduty`).<br>- Gán target tới hàm Lambda để phân tích và làm giàu payload cảnh báo GuardDuty đẩy về SNS/DynamoDB. | 04/08/2026 | 04/08/2026 | <https://000018.awsstudygroup.com> |
| 4 | - Tái thực thi Kịch bản Tấn công Cloud 8–12:<br>&emsp;+ Đăng nhập Root không MFA<br>&emsp;+ Dò quét quyền IAM<br>&emsp;+ Tạo persistence key IAM<br>&emsp;+ Rút dữ liệu S3 & công khai bucket.<br>- Ghi nhận mốc thời gian phát hiện giữa Elastic SIEM (KQL) và GuardDuty. | 05/08/2026 | 05/08/2026 | Cloud Attack Simulation Suite |
| 5 | - Thu thập dữ liệu đánh giá thực nghiệm:<br>&emsp;+ Độ trễ KQL tùy chỉnh: ~4–13 phút SIEM poll.<br>&emsp;+ Giới hạn baseline của GuardDuty: Do bật GuardDuty muộn (ngày 26/07), dịch vụ chỉ mới hoạt động ~3 ngày trước khi thử nghiệm (29–30/07). Do đó, GuardDuty chưa có đủ thời gian tích lũy baseline cho các bộ phát hiện bất thường (IAM recon, IAM persistence, S3 exfiltration), trong khi phát hiện dạng chữ ký (`Policy:S3/BucketAnonymousAccessGranted`) vẫn báo động nhanh chóng sau ~6.7 phút.<br>&emsp;+ Bài toán chi phí: Quy tắc KQL dựa trên SQS/Lambda Free Tier ($0); GuardDuty chuyển sang tính phí sau khi hết trial. | 06/08/2026 | 06/08/2026 | Empirical Testing Data |
| 6 | - Viết báo cáo đánh giá so sánh (`guardduty-comparison.md`).<br>- Chủ động tắt GuardDuty detector và S3 Protection trước khi hết hạn trial để giữ nguyên chi phí AWS ở mức $0. | 07/08/2026 | 07/08/2026 | <https://000098.awsstudygroup.com> |

### Kết quả đạt được Tuần 8

* Tích hợp thành công dịch vụ phát hiện đe dọa tự động AWS GuardDuty vào luồng cảnh báo serverless qua EventBridge.
* Thực hiện so sánh đối chứng thực nghiệm giữa các quy tắc KQL tùy chỉnh và cảnh báo tự động của AWS GuardDuty.
* Đánh giá thực nghiệm cơ chế phát hiện: Ghi nhận việc bật GuardDuty muộn (26/07) khiến các bộ phát hiện bất thường bằng máy học (ML) chưa có đủ dữ liệu baseline để cảnh báo các kịch bản hành vi, qua đó làm nổi bật ưu thế của luật KQL tùy chỉnh trên SIEM trong việc cảnh báo tức thì mà không cần thời gian học baseline.
* Hoàn thành báo cáo phân tích so sánh chuyên sâu `guardduty-comparison.md`.
* Đảm bảo kỷ luật chi phí nghiêm ngặt khi chủ động tắt GuardDuty trước khi hết trial, duy trì chi phí AWS ở mức $0.
