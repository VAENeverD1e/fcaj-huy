---
title: "Worklog Tuần 1"
date: 2026-06-15
weight: 1
chapter: false
pre: " <b> 1.1. </b> "
---
### Mục tiêu Tuần 1

* Hoàn thành thủ tục tiếp nhận thực tập FCAJ, nắm rõ quy định và lộ trình thực tập.
* Học các kiến thức AWS cơ bản: Điều hướng Console, cấu hình AWS CLI, quản lý tài khoản IAM, bảo mật MFA và cảnh báo ngân sách Free Tier.
* Nắm vững các thao tác trên Amazon EC2: Tạo instance, chọn AMI, gắn ổ cứng EBS và cấu hình Security Group.
* Lập phạm vi dự án và chuẩn bị thiết kế kiến trúc cho Phòng thí nghiệm phát hiện đe dọa Endpoint.

### Các công việc triển khai trong tuần

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Tham gia buổi định hướng FCAJ và kết nối với các thành viên.<br>- Đọc kỹ nội quy và quy định đơn vị thực tập.<br>- Xác định lộ trình 9 tuần kết hợp học AWS và làm dự án SOC Lab. | 15/06/2026 | 15/06/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 3 | - Tạo tài khoản AWS Free Tier.<br>- Bảo mật tài khoản Root: bật MFA, tạo user IAM Admin riêng.<br>- Cấu hình AWS Zero-Spend Budget Alarm và cảnh báo chi phí CloudWatch. | 16/06/2026 | 16/06/2026 | <https://000001.awsstudygroup.com><br><https://000007.awsstudygroup.com> |
| 4 | - Cài đặt và cấu hình AWS CLI trên máy trạm (`aws configure`).<br>- Thực hành các lệnh CLI: quản lý key pair, liệt kê region, kiểm tra EC2 instance. | 17/06/2026 | 17/06/2026 | <https://000011.awsstudygroup.com> |
| 5 | - Tìm hiểu các khái niệm EC2: Instance types (t2.micro/t3.micro), AMI, EBS storage types.<br>- Tìm hiểu quản lý SSH key pair và luật Security Group. | 18/06/2026 | 18/06/2026 | <https://000004.awsstudygroup.com> |
| 6 | - Khởi tạo thử nghiệm EC2 Linux & Windows.<br>- Kết nối SSH/RDP, gắn và tháo ổ đĩa EBS.<br>- Phác thảo kiến trúc phòng thí nghiệm phát hiện đe dọa Endpoint. | 19/06/2026 | 19/06/2026 | <https://000048.awsstudygroup.com> |

### Kết quả đạt được Tuần 1

* Hoàn thành hội nhập chương trình thực tập FCAJ và thống nhất kế hoạch thực hiện dự án.
* Thiết lập môi trường AWS Free Tier an toàn với hệ thống cảnh báo ngân sách $0 spend.
* Thành thạo cài đặt AWS CLI, quản lý chứng thực và quản trị song song qua Web Console/CLI.
* Nắm vững kỹ năng khởi tạo, cấu hình và kết nối đến EC2 instance cùng ổ đĩa EBS.
* Hoàn thành bản phác thảo kiến trúc sơ bộ cho phòng thí nghiệm SOC.
