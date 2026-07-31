---
title: "Worklog Tuần 12"
date: 2024-01-01
weight: 12
chapter: false
pre: " <b> 1.12. </b> "
---
### Mục tiêu Tuần 12

* Kiểm thử tích hợp toàn diện trên cả hai luồng phát hiện: Phân tích log trên Elastic SIEM và Đẩy cảnh báo thời gian thực dạng Serverless.
* Thực hiện kiểm toán tài chính nghiêm ngặt xác nhận chi phí AWS đạt mức xấp xỉ $0 trong suốt 12 tuần thực tập.
* Hoàn thiện toàn bộ tài liệu kỹ thuật, sơ đồ kiến trúc hệ thống (`architecture.png`, `cloud-architecture.png`), hướng dẫn thực thi kịch bản, bản tự đánh giá và hồ sơ năng lực thực tập.
* Báo cáo và bảo vệ kết quả dự án cuối kỳ trước hội đồng đánh giá thực tập First Cloud AI Journey (FCAJ).

### Các công việc triển khai trong tuần

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Tiến hành kiểm thử tích hợp toàn bộ hệ thống:<br>&emsp;+ Chạy toàn bộ 12 kịch bản tấn công (7 kịch bản Endpoint + 5 kịch bản AWS Cloud).<br>&emsp;+ Xác minh thông báo cảnh báo kép: Email SNS gửi tức thì (~2s) và quy tắc SIEM phân tích log chuyên sâu (~5 min).<br>&emsp;+ Xác minh các chỉ số hiển thị chính xác trên giao diện Automation Ops Dashboard. | 31/08/2026 | 31/08/2026 | End-to-End Integration Test Suite |
| 3 | - Kiểm toán tài chính AWS:<br>&emsp;+ Kiểm tra AWS Cost Explorer và cảnh báo ngân sách CloudWatch Billing Alarms.<br>&emsp;+ Xác nhận tổng chi phí AWS trong toàn bộ chu kỳ dự án duy trì ở mức $0 (không phát sinh phí GuardDuty hay AWS Config). | 01/09/2026 | 01/09/2026 | <https://000007.awsstudygroup.com> |
| 4 | - Đính kèm sơ đồ kiến trúc chất lượng cao (`architecture.png` cho phòng thí nghiệm Windows và `cloud-architecture.png` cho luồng AWS) vào trang tài liệu dự án.<br>- Cập nhật hoàn thiện phần 7 (Timeline) và phần 10 (Kết quả đạt được) trong bản đề xuất.<br>- Chuẩn hóa Module 5.2 Tiền đề & Chuẩn bị: làm rõ mô hình ELK VM tự quản lý, phân định rõ vai trò máy ảo nạn nhân với máy host quản trị, và tối ưu hóa bảng kiểm tra phiên bản công cụ. | 02/09/2026 | 02/09/2026 | Documentation Management |
| 5 | - Viết báo cáo tự đánh giá kết quả thực tập (`content/6-Self-evaluation/`).<br>- Rà soát tài liệu đóng góp ý kiến sinh viên (`content/7-Feedback/`).<br>- Chạy thử nghiệm đóng gói tài liệu Hugo (`hugo`) đảm bảo hiển thị chuẩn trên tất cả các trang. | 03/09/2026 | 03/09/2026 | <https://cloudjourney.awsstudygroup.com/8-fcjworkforce/> |
| 6 | - Báo cáo bảo vệ dự án kỹ thuật cuối kỳ trước các mentor FCAJ.<br>- Xuất bản mã nguồn hoàn chỉnh của SOC Documentation Portal.<br>- Hoàn thành xuất sắc chương trình thực tập First Cloud AI Journey (FCAJ). | 04/09/2026 | 04/09/2026 | <https://cloudjourney.awsstudygroup.com/> |

### Kết quả đạt được Tuần 12

* Xác minh sự thành công và độ tin cậy của mô hình cảnh báo kép trên toàn bộ 12 kịch bản tấn công an ninh mạng.
* Chứng minh kỷ luật tài chính nghiêm ngặt: Duy trì tổng chi phí AWS ở mức $0 cho toàn bộ lộ trình 12 tuần của dự án.
* Xuất bản bộ tài liệu dự án hoàn chỉnh bao gồm 12 hướng dẫn kịch bản tấn công, 4 báo cáo săn đe dọa, 1 báo cáo so sánh GuardDuty, bộ mã nguồn Terraform IaC, toàn bộ mã nguồn Ops Dashboard, và bộ hướng dẫn tiền đề workshop Module 5.2 chuẩn hóa.
* Đính kèm sơ đồ kiến trúc hệ thống trực quan vào trang cổng thông tin tài liệu SOC Portal.
* Hoàn thành xuất sắc chương trình thực tập First Cloud AI Journey (FCAJ).
