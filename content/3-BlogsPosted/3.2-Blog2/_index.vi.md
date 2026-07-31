---
title: "Blog 2"
date: 2026-07-31
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
---

# AWS SOC DETECTION LAB — TỰ ĐỘNG HÓA PHÁT HIỆN MỐI ĐE DỌA & VẬN HÀNH PIPELINE TRÊN AWS

Trong môi trường Cloud Security, việc phát hiện sớm và phản ứng tức thì với các hành vi bất thường là yếu tố sống còn. Dự án **AWS SOC Detection Lab** được thiết kế như một hệ thống phát hiện sự cố bảo mật tự động, kết hợp với một **Operations Dashboard** giúp giám sát trực quan toàn bộ quá trình vận hành đó.

## Mục tiêu cốt lõi của dự án

Dự án tập trung giải quyết 2 bài toán chính:
1. **Tự động hóa phát hiện mối đe dọa (Automation Threat Detection)**: Theo dõi các thao tác nguy hiểm trên tài khoản AWS và gửi cảnh báo ngay lập tức.
2. **Giám sát sức khỏe Pipeline (Pipeline Observability)**: Cung cấp giao diện Dashboard đo lường độ tin cậy, hiệu năng và trạng thái hoạt động của chính hệ thống cảnh báo.

---

## Luồng hoạt động tổng quan

Hệ thống vận hành theo mô hình Event-Driven hoàn toàn serverless:
1. **Phát hiện**: Khi xuất hiện hành vi nhạy cảm (như mở public S3 Bucket, tạo IAM User bất thường, đổi policy), CloudTrail và GuardDuty sẽ ghi nhận.
2. **Lọc & Kích hoạt**: EventBridge bắt sự kiện theo thời gian thực và kích hoạt ngay AWS Lambda.
3. **Xử lý & Cảnh báo**: Lambda phân loại mức độ nghiêm trọng (`CRITICAL`, `HIGH`, `MEDIUM`), phát cảnh báo qua SNS Email và lưu thông số vào DynamoDB.
4. **Trực quan hóa**: Operations Dashboard truy vấn dữ liệu để hiển thị bức tranh toàn cảnh về tình trạng hệ thống.

---

## Điểm đặc biệt của Operations Dashboard

Khác với các công cụ SIEM thông thường (chỉ tập trung phân tích nội dung cảnh báo), **Operations Dashboard** trong dự án này đóng vai trò như một bảng điều khiển kỹ thuật (NOC/SOC Dashboard):

* **Đo lường độ tin cậy**: Hiển thị tổng số sự kiện, tỷ lệ thành công (`SUCCESS`) và lỗi (`ERROR`).
* **Theo dõi hiệu năng**: Thống kê thời gian xử lý (latency) của Lambda và SNS.
* **Tối ưu chi phí**: Tích hợp cơ chế Caching và theo dõi định mức AWS Free Tier để đảm bảo hệ thống vận hành tối ưu nhất.

---

## Kết luận

**AWS SOC Detection Lab** là sự kết hợp hài hòa giữa **Bảo mật (Security)**, **Tự động hóa (Automation)** và **Vận hành (Operations)**. Hệ thống mang lại cái nhìn toàn diện giúp đội ngũ kĩ thuật vừa đảm bảo an toàn cho hạ tầng Cloud, vừa nắm rõ trạng thái hoạt động của hệ thống giám sát.

**🔗 Tham khảo thêm:**
- [Soc Detection Lab](https://github.com/VAENeverD1e/soc-detection-lab)
- [Operations Dashboard](https://github.com/KoangWy/ops-dashboard)