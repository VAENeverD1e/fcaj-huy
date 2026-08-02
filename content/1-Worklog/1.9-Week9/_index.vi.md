---
title: "Worklog Tuần 9"
date: 2024-01-01
weight: 9
chapter: false
pre: " <b> 1.9. </b> "
---
### Mục tiêu Tuần 9

* Thực hiện kiểm thử tích hợp toàn bộ hệ thống trên cả hai đường ống phát hiện: Phân tích log chuyên sâu trên Elastic SIEM và cảnh báo thời gian thực serverless.
* Thực hiện kiểm toán tài chính nghiêm ngặt xác nhận chi phí AWS đạt mức xấp xỉ $0 trong suốt 9 tuần thực tập.
* Hoàn thiện toàn bộ tài liệu kỹ thuật, sơ đồ kiến trúc (`architecture.png`, `cloud-architecture.png`), hướng dẫn thực thi kịch bản, báo cáo tự đánh giá và hồ sơ năng lực thực tập.
* Báo cáo và bảo vệ kết quả dự án trước hội đồng đánh giá chương trình thực tập First Cloud AI Journey (FCAJ).

### Các công việc triển khai trong tuần

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Tiến hành kiểm thử tích hợp toàn bộ hệ thống:<br>&emsp;+ Chạy toàn bộ 12 kịch bản tấn công (7 kịch bản Endpoint + 5 kịch bản AWS Cloud).<br>&emsp;+ Xác minh thông báo cảnh báo kép: Email SNS gửi tức thì (~2s) và quy tắc SIEM phân tích log chuyên sâu (~5 min).<br>&emsp;+ Xác minh các chỉ số hiển thị chính xác trên giao diện Automation Ops Dashboard. | 10/08/2026 | 10/08/2026 | End-to-End Integration Test Suite |
| 3 | - Kiểm toán tài chính AWS:<br>&emsp;+ Kiểm tra AWS Cost Explorer và CloudWatch Billing Alarms.<br>&emsp;+ Xác nhận tổng chi phí AWS trong toàn bộ lộ trình 9 tuần đạt mức $0 (không phát sinh phí GuardDuty hay Config). | 11/08/2026 | 11/08/2026 | <https://000007.awsstudygroup.com> |
| 4 | - Tích hợp sơ đồ kiến trúc chất lượng cao vào bộ tài liệu dự án.<br>- Hoàn thiện chỉnh sửa đề xuất tại Mục 7 (Lộ trình thời gian) và Mục 10 (Kết quả kỳ vọng).<br>- Chuẩn hóa tài liệu tiền đề Workshop Module 5.2. | 12/08/2026 | 12/08/2026 | Documentation Management |
| 5 | - Viết báo cáo tự đánh giá thực tập (`content/6-Self-evaluation/`).<br>- Tổng hợp tài liệu phản hồi học viên (`content/7-Feedback/`).<br>- Kiểm thử biên dịch cổng tài liệu Hugo (`hugo`) đảm bảo mọi trang hiển thị chính xác. | 13/08/2026 | 13/08/2026 | <https://cloudjourney.awsstudygroup.com/8-fcjworkforce/> |
| 6 | - Báo cáo bảo vệ dự án kỹ thuật trước đội ngũ mentor FCAJ.<br>- Xuất bản toàn bộ mã nguồn cổng tài liệu SOC.<br>- Hoàn thành xuất sắc chương trình thực tập FCAJ. | 14/08/2026 | 14/08/2026 | <https://cloudjourney.awsstudygroup.com/> |

### Kết quả đạt được Tuần 9

* Xác minh sự thành công và độ tin cậy của mô hình cảnh báo kép trên toàn bộ 12 kịch bản tấn công an ninh mạng.
* Chứng minh kỷ luật tài chính nghiêm ngặt: Duy trì tổng chi phí AWS ở mức $0 cho toàn bộ lộ trình 9 tuần của dự án.
* Xuất bản bộ tài liệu dự án hoàn chỉnh bao gồm 12 hướng dẫn kịch bản tấn công, 4 báo cáo săn đe dọa, 1 báo cáo so sánh GuardDuty, bộ mã nguồn Terraform IaC, toàn bộ mã nguồn Ops Dashboard, và bộ hướng dẫn tiền đề workshop Module 5.2 chuẩn hóa.
* Nhúng thành công sơ đồ kiến trúc trực quan vào trang cổng tài liệu SOC.
* Hoàn thành chương trình thực tập First Cloud AI Journey (FCAJ) với kết quả xuất sắc.
