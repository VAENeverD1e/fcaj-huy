---
title: "Worklog Tuần 2"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---
### Mục tiêu Tuần 2

* Tìm hiểu các khái niệm mạng AWS VPC: VPC CIDR block, public/private subnet, Internet Gateway, Security Group và Network ACL.
* Xây dựng môi trường phòng thí nghiệm phát hiện đe dọa trên Windows Server / Windows 11.
* Cài đặt Elastic Agent & Sysmon với cấu hình SwiftOnSecurity để thu thập toàn bộ dữ liệu telemetry từ máy trạm.
* Thực thi 7 kịch bản mô phỏng tấn công Endpoint sử dụng Atomic Red Team (trích xuất credential, leo thang đặc quyền, duy trì truy cập, thực thi lệnh).
* Phát triển các quy tắc phát hiện KQL tùy chỉnh và viết 4 báo cáo săn đe dọa (threat hunt report).

### Các công việc triển khai trong tuần

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Tìm hiểu mạng AWS VPC: public/private subnet, bảng tuyến đường (route table), Security Group vs NACL.<br>- Cấu hình quy tắc cách ly mạng cho môi trường mô phỏng tấn công. | 22/06/2026 | 22/06/2026 | <https://000003.awsstudygroup.com><br><https://000092.awsstudygroup.com> |
| 3 | - Khởi tạo môi trường Windows 11 / Windows Server.<br>- Cài đặt Sysmon v15+ với tệp cấu hình SwiftOnSecurity.<br>- Cài đặt Elastic Agent và kết nối về Elastic SIEM. | 23/06/2026 | 23/06/2026 | <https://000093.awsstudygroup.com><br><https://github.com/SwiftOnSecurity/sysmon-config> |
| 4 | - Thực thi kịch bản tấn công Endpoint 1–3:<br>&emsp;+ Khởi tạo tiến trình Windows (T1059.001 - PowerShell Execution)<br>&emsp;+ Trích xuất bộ nhớ LSASS (T1003.001 - LSASS Memory Dumping via Mimikatz/ProcDump)<br>&emsp;+ Leo thang đặc quyền (T1548.002 - Bypass UAC). | 24/06/2026 | 24/06/2026 | Thư viện Atomic Red Team |
| 5 | - Thực thi kịch bản tấn công Endpoint 4–7:<br>&emsp;+ Duy trì truy cập qua Registry Run Keys (T1547.001)<br>&emsp;+ Khởi tạo Scheduled Task (T1053.005)<br>&emsp;+ Né tránh phòng thủ (T1562.001 - Impair Defenses)<br>&emsp;+ Thu thập thông tin hệ thống (T1082 - System Information Discovery). | 25/06/2026 | 25/06/2026 | Thư viện Atomic Red Team |
| 6 | - Kiểm tra dữ liệu telemetry đẩy về Elastic SIEM (`winlogbeat-*` & `logs-windows.*`).<br>- Viết và xác minh 7 quy tắc phát hiện KQL trên SIEM.<br>- Lập 4 báo cáo săn đe dọa (threat hunt report) ghi lại kỹ thuật tấn công, log chứng cứ và khuyến nghị khắc phục. | 26/06/2026 | 26/06/2026 | Elastic Security Docs |

### Kết quả đạt được Tuần 2

* Nắm vững kiến thức mạng cơ bản trên AWS VPC, định tuyến subnet và cấu hình tường lửa Security Group.
* Xây dựng thành công phòng thí nghiệm phát hiện đe dọa trên Windows tích hợp Sysmon và Elastic Agent.
* Mô phỏng thành công 7 kịch bản tấn công thực tế trên máy trạm bằng framework Atomic Red Team.
* Xây dựng 7 quy tắc KQL chất lượng cao phát hiện hành vi tạo tiến trình bất thường, trích xuất tài khoản và duy trì truy cập.
* Hoàn thành 4 báo cáo săn đe dọa chuyên sâu, thiết lập vòng phản hồi săn đe dọa - gia cố hệ thống.
