---
title: "Worklog Tuần 4"
date: 2026-07-06
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---
### Mục tiêu Tuần 4

* Phân tích cấu trúc JSON của sự kiện CloudTrail (`userIdentity`, `eventName`, `eventSource`, `requestParameters`, `sourceIPAddress`).
* Tìm hiểu các kỹ thuật tấn công Cloud, ma trận MITRE ATT&CK cho Cloud.
* Thực thi 5 kịch bản mô phỏng tấn công Cloud thực tế sử dụng tài khoản IAM thử nghiệm (`lab-attacker`).
* Phát triển các quy tắc phát hiện KQL tùy chỉnh trên Elastic SIEM để phát hiện hành vi tấn công Cloud theo thời gian thực.
* Xác minh quy trình dọn dẹp môi trường tự động để khôi phục trạng thái an toàn ngay sau khi thử nghiệm.

### Các công việc triển khai trong tuần

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Nghiên cứu tài liệu ma trận MITRE ATT&CK Cloud.<br>- Tạo tài khoản IAM thử nghiệm (`lab-attacker`) và khởi tạo access key phục vụ tấn công.<br>- Phân tích chi tiết cấu trúc log sự kiện xác thực và gọi API trong CloudTrail. | 06/07/2026 | 06/07/2026 | <https://000044.awsstudygroup.com><br>MITRE ATT&CK Cloud Matrix |
| 3 | - Thực thi Kịch bản Tấn công Cloud 8: Đăng nhập tài khoản AWS Root không có MFA.<br>- Thực thi Kịch bản Tấn công Cloud 9: Dò quét quyền hạn IAM & Liệt kê chính sách (`GetAccountSummary`, `ListUsers`, `ListRoles`). | 07/07/2026 | 07/07/2026 | <https://000011.awsstudygroup.com> |
| 4 | - Thực thi Kịch bản Tấn công Cloud 10: Tạo backdoor duy trì quyền truy cập IAM (tạo access key ẩn `CreateAccessKey` và gán quyền admin `AttachUserPolicy`).<br>- Thực thi Kịch bản Tấn công Cloud 11: Trích xuất dữ liệu S3 số lượng lớn (`GetObject` trên các bucket nhạy cảm). | 08/07/2026 | 08/07/2026 | <https://000069.awsstudygroup.com> |
| 5 | - Thực thi Kịch bản Tấn công Cloud 12: Cấu hình công khai S3 bucket (thay đổi ACL / gỡ bỏ Block Public Access qua `PutBucketAcl` / `DeletePublicAccessBlock`).<br>- Chạy kịch bản dọn dẹp tự động để khôi phục lại quyền hạn ban đầu. | 09/07/2026 | 09/07/2026 | <https://000030.awsstudygroup.com> |
| 6 | - Truy vấn log CloudTrail trên Elastic SIEM để xác định chữ ký audit của từng API call.<br>- Xây dựng, thử nghiệm và tối ưu 5 quy tắc phát hiện KQL tùy chỉnh cho đăng nhập Root, dò quét IAM, tạo credential ẩn, rút dữ liệu S3 và công khai bucket S3. | 10/07/2026 | 10/07/2026 | Elastic Security Rule Engine |

### Kết quả đạt được Tuần 4

* Hiểu rõ cấu trúc dữ liệu log audit CloudTrail JSON và chữ ký API tương ứng của từng hành vi.
* Mô phỏng thành công 5 kịch bản tấn công Cloud bao gồm chiếm dụng quyền đăng nhập, dò quét IAM, duy trì truy cập và rút dữ liệu S3.
* Xây dựng 5 quy tắc phát hiện KQL tùy chỉnh chất lượng cao trên Elastic SIEM với tỷ lệ cảnh báo giả rất thấp.
* Xác minh thành công quy trình khôi phục môi trường an toàn sau khi thử nghiệm.
* Hoàn thiện công cụ phát hiện lai cho cả máy trạm Windows và hạ tầng đám mây AWS.
