---
title: "Các bài blogs đã đăng"
date: 2026-07-31
weight: 3
chapter: false
pre: " <b> 3. </b> "
---

Tại đây sẽ là phần liệt kê, giới thiệu các blogs mà các bạn đã đăng trên [AWS Study Group](https://www.facebook.com/groups/awsstudygroupfcj).

### [Blog 1 - AMAZON EVENTBRIDGE — TRÁI TIM CỦA KIẾN TRÚC EVENT-DRIVEN SECURITY TRÊN AWS](3.1-Blog1/_index.vi.md)
Blog này giới thiệu Amazon EventBridge — dịch vụ serverless event bus của AWS đóng vai trò trung tâm điều phối trong kiến trúc phát hiện và tự động hóa phản ứng bảo mật (Security SOAR). Bài viết phân tích cách EventBridge bắt sự kiện CloudTrail theo thời gian thực, kỹ thuật lọc sự kiện bằng JSON pattern, giải pháp xử lý sự kiện đa vùng (Multi-Region) và tại sao mô hình zero-polling của EventBridge vượt trội hơn cách polling truyền thống trong bối cảnh bảo mật.
[Hình ảnh](../../static/images/3-BlogsPosted/blog1.png)

### [Blog 2 - AWS SOC DETECTION LAB — TỰ ĐỘNG HÓA PHÁT HIỆN MỐI ĐE DỌA & VẬN HÀNH PIPELINE TRÊN AWS](3.2-Blog2/_index.vi.md)
Blog này tổng quan về dự án **AWS SOC Detection Lab** — hệ thống phát hiện mối đe dọa tự động kết hợp Operations Dashboard. Bài viết tập trung trình bày đơn giản luồng vận hành từ phát hiện sự kiện (CloudTrail/GuardDuty) đến xử lý tự động (EventBridge, Lambda, SNS, DynamoDB) và vai trò giám sát sức khỏe pipeline của Dashboard.
[Hình ảnh](../../static/images/3-BlogsPosted/blog2.png)

### [Blog 3 - TERRAFORM IaC — TẠI SAO NÊN DÙNG INFRASTRUCTURE AS CODE CHO AWS SECURITY LAB?](3.3-Blog3/_index.vi.md)
Blog này lý giải tại sao Terraform là lựa chọn thiết yếu khi xây dựng Security Lab trên AWS với nhiều dịch vụ liên kết. Bài viết phân tích 3 lợi ích cốt lõi: Reproducibility (tái tạo hạ tầng chỉ với một lệnh), Security & Least Privilege (review IAM policy như review code), và Dependency Management tự động. Kèm theo là bảng Best Practices thực chiến và ví dụ HCL code minh hoạ trực tiếp từ dự án SOC Detection Lab.
[Hình ảnh](../../static/images/3-BlogsPosted/blog3.png)