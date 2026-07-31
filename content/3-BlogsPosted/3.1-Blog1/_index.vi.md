---
title: "Blog 1"
date: 2026-07-31
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

# AMAZON EVENTBRIDGE — TRÁI TIM CỦA KIẾN TRÚC EVENT-DRIVEN SECURITY TRÊN AWS

Amazon EventBridge là một dịch vụ **serverless event bus** được quản lý hoàn toàn bởi AWS, cho phép bạn kết nối các ứng dụng với nhau thông qua các sự kiện (events) theo thời gian thực. Trong các hệ thống phát hiện và phản ứng bảo mật (Security Detection & Response), EventBridge đóng vai trò như "trái tim" điều phối toàn bộ luồng xử lý tự động hóa.

## EventBridge hoạt động như thế nào?

Khi một người dùng thực hiện thao tác trên AWS (ví dụ: tạo IAM User mới, thay đổi chính sách S3 Bucket), CloudTrail sẽ ghi lại sự kiện đó. EventBridge liên tục lắng nghe các sự kiện này và so khớp chúng với các **Event Rules** đã được định nghĩa trước. Nếu khớp, EventBridge sẽ ngay lập tức kích hoạt (trigger) một Target — có thể là Lambda Function, SQS Queue, SNS Topic, v.v.

```
CloudTrail API Call
        │
        ▼
EventBridge Event Bus
        │ (So khớp Event Pattern)
        ▼
Event Rule Match
        │
        ▼
Lambda Function (Target)
```

## Tại sao EventBridge phù hợp cho Security Automation?

**1. Near Real-time Detection (Phát hiện gần như tức thì)**
Độ trễ từ khi sự kiện xảy ra đến khi Lambda được kích hoạt thường chỉ trong vài giây, nhanh hơn rất nhiều so với việc polling định kỳ CloudTrail logs từ S3.

**2. Event Pattern Filtering (Lọc sự kiện chính xác)**
Bạn có thể định nghĩa chính xác sự kiện nào cần bắt bằng cú pháp JSON pattern. Ví dụ, chỉ bắt các API Call có `eventName` là `CreateUser` hoặc `AttachUserPolicy`:

```json
{
  "source": ["aws.iam"],
  "detail-type": ["AWS API Call via CloudTrail"],
  "detail": {
    "eventName": ["CreateUser", "AttachUserPolicy", "CreateAccessKey"]
  }
}
```

**3. Multi-Region Support (Hỗ trợ đa vùng)**
Một điểm đặc biệt quan trọng: AWS phát ra các sự kiện IAM và Console Sign-in toàn cục (global) tại vùng `us-east-1`, bất kể tài nguyên thực tế của bạn nằm ở đâu. Giải pháp là tạo Event Rule riêng tại `us-east-1` và trỏ đến Lambda Function nằm ở vùng chính (ví dụ: `ap-southeast-2`).

**4. Zero-Polling Architecture (Không cần polling)**
Thay vì lên lịch kiểm tra log định kỳ (tốn RCU DynamoDB và Lambda invocations), EventBridge chỉ kích hoạt Lambda khi có sự kiện thật sự xảy ra — tiết kiệm chi phí đáng kể.

## Điểm nổi bật cần lưu ý

* Một Event Rule có thể có nhiều **Targets** (tối đa 5), ví dụ vừa gọi Lambda vừa đẩy sang SQS cùng lúc.
* Nên bật **Dead Letter Queue (DLQ)** cho Target Lambda để không bị mất sự kiện khi Lambda bị lỗi.
* EventBridge đảm bảo **at-least-once delivery** — Lambda có thể được gọi nhiều hơn một lần cho cùng một sự kiện, vì vậy cần đảm bảo handler của bạn **idempotent**.
* **Chi phí Custom Events**: AWS tính phí **$1.00/triệu events** từ event đầu tiên — **không có Free Tier cho custom events**. Tuy nhiên, **AWS Management Events** (bao gồm CloudTrail API calls) được ingest vào default event bus **hoàn toàn miễn phí**, đây chính xác là những gì dự án của nhóm mình sử dụng. EventBridge Scheduler có free tier 14 triệu invocations/tháng (nhưng đây là tính năng khác).

Với EventBridge, bạn có thể xây dựng một hệ thống phát hiện mối đe dọa AWS hoàn toàn serverless, phản ứng tức thì và không cần quản lý server — đây chính là nền tảng của mô hình **AWS-Native SOAR (Security Orchestration, Automation and Response)**.

**🔗 Tham khảo thêm:**
- [Amazon EventBridge Documentation](https://docs.aws.amazon.com/eventbridge/latest/userguide/what-is-amazon-eventbridge.html)
- [EventBridge Event Patterns](https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-event-patterns.html)