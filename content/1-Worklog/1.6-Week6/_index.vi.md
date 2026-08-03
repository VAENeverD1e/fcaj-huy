---
title: "Worklog Tuần 6"
date: 2026-07-20
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---
### Mục tiêu Tuần 6

* Tìm hiểu các dịch vụ quét an ninh thụ động IAM Access Analyzer, truy vấn log SQL serverless Amazon Athena và phát hiện đe dọa managed AWS GuardDuty.
* Xây dựng bộ 4 câu lệnh truy vấn SQL SOC nòng cốt trong Amazon Athena Console để săn đe dọa trên tệp log CloudTrail S3 thô.
* Đánh giá kết quả cảnh báo từ GuardDuty so với luật KQL tùy chỉnh trên SIEM và hoàn thiện báo cáo so sánh đối chứng (`guardduty-comparison.md`).
* Triển khai giao diện điều khiển Frontend (`frontend/`) bằng React, Vite và thiết kế giao diện tối (Dark Mode) hiện đại.
* Tích hợp các component React Frontend (`PipelineStatusWidget`, `CostTrackerWidget`, `LatencyChart`, `AlertFeedTable`) với REST API Backend qua cơ chế tự động lấy dữ liệu (30 giây/lần).

### Các công việc triển khai trong tuần

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Bật IAM Access Analyzer với ranh giới trust zone là tài khoản AWS.<br>- Cấu hình Athena database & workgroup trên S3 bucket lưu log CloudTrail. | 20/07/2026 | 20/07/2026 | <https://000030.awsstudygroup.com> |
| 3 | - Xây dựng và thực thi 4 câu lệnh truy vấn SQL SOC trong Athena Query Editor.<br>- Bật AWS GuardDuty detector trong chế độ dùng thử 30 ngày (Free Trial). | 21/07/2026 | 21/07/2026 | <https://000106.awsstudygroup.com> |
| 4 | - Thu thập dữ liệu thực nghiệm so sánh độ trễ KQL (~4–13 min) với thời gian phát hiện của GuardDuty.<br>- Viết báo cáo đánh giá so sánh chuyên sâu (`guardduty-comparison.md`) và chủ động tắt GuardDuty trước khi hết trial để giữ chi phí $0. | 22/07/2026 | 22/07/2026 | <https://000098.awsstudygroup.com> |
| 5 | - Khởi tạo dự án ứng dụng React sử dụng Vite (`npx create-vite@latest frontend --template react`).<br>- Thiết lập hệ thống CSS design system, màu sắc Dark Mode và phát triển các UI component chính (`PipelineStatusWidget`, `CostTrackerWidget`, `LatencyChart`, `AlertFeedTable`). | 23/07/2026 | 23/07/2026 | <https://000079.awsstudygroup.com> |
| 6 | - Tích hợp React Frontend với các REST API Endpoint Backend (`http://localhost:5000/api`).<br>- Thêm cơ chế tự động làm mới dữ liệu sau mỗi 30 giây và kiểm thử đóng gói ứng dụng web (`npm run build`). | 24/07/2026 | 24/07/2026 | Web App Testing & Build |

### Kết quả đạt được Tuần 6

* Bật thành công AWS IAM Access Analyzer tự động phát hiện cấu hình sai thụ động ở mức chi phí $0.
* Xây dựng bộ 4 câu lệnh truy vấn SQL điều tra sự cố thực tế trong Amazon Athena phục vụ săn đe dọa trên tệp log CloudTrail S3 thô.
* Thực hiện so sánh đối chứng thực nghiệm giữa luật KQL tùy chỉnh và cảnh báo GuardDuty, hoàn thiện báo cáo `guardduty-comparison.md`.
* Xây dựng giao diện web ứng dụng hiện đại chuẩn Dark Theme bằng React và Vite trong thư mục `frontend/`.
* Kết nối mượt mà React Frontend với Python Backend API kèm biểu đồ độ trễ trực quan và cơ chế làm mới telemetry mỗi 30 giây.
