---
title: "Event 3"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 4.3. </b> "
---

# Bài thu hoạch "Agentic AI Buildweek Hackathon"

### Mục Đích Của Sự Kiện

- Tạo sân chơi thực chiến cho các đội ngũ kỹ sư trẻ xây dựng các giải pháp ứng dụng AI Agent (Agentic AI) trên hạ tầng AWS.
- Khuyến khích tư duy đổi mới sáng tạo, chuyển đổi từ mô hình phát triển phần mềm truyền thống sang tự động hóa bằng AI Agents.
- Cung cấp trải nghiệm kết nối (Networking), làm việc nhóm dưới áp lực cao (24h Hackathon) và nhận phản hồi từ các chuyên gia hàng đầu (AWS, VCs).

### Danh Sách Diễn Giả & Khách Mời

- **Joseph Marazzotta** - Head of Technology @ AWS ASEAN
- **Nguyễn Gia Hưng** - Head of Solution Architect @ AWS Vietnam (Founder @ AWS First Cloud AI Journey)
- **Đội ngũ Diễn giả từ các Đội thi Giành Giải**: One Team (Giải Nhất), Signal Scout (Giải Nhì), Team Plan, Team 3K, Team Six Pillar.

### Nội Dung Nổi Bật

#### Khai Mạc & Thông Điệp Từ Đội Ngũ Lãnh Đạo AWS (Joseph Marazzotta)

- **Tốc độ thay đổi kỷ nguyên AI**: Sự dịch chuyển từ chu kỳ release theo quý/tuần trước đây sang tự động hóa hoàn toàn theo từng phút bằng AI Agents.
- **Tư duy đổi mới (Mindset)**: Dũng cảm thách thức những rào cản truyền thống, học tập suốt đời (Lifelong learning) để định hình tương lai công nghệ tại Việt Nam và khu vực.

#### Dự Án 1: KFC AI Force Agent - Conversational Ordering (One Team - Giải Nhất)

- **Bối cảnh & Bài toán**: Khắc phục các thất bại của chatbot truyền thống (Hallucination, nhầm đơn hàng) và loại bỏ rào cản tải app/tạo tài khoản khi đặt hàng F&B.
- **Giải pháp**: Xây dựng Agent đặt hàng đa kênh (Zalo/WhatsApp) cho KFC. Khách hàng giao tiếp tự nhiên trực tiếp trên ứng dụng chat không cần chuyển cảnh.
- **Kiến trúc Kỹ thuật**: Sử dụng **AWS Bedrock Agent Core** (lưu trữ session memory), WAF bảo mật traffic, kết hợp TinyFish/Apify cào dữ liệu menu thực tế.
- **Hiệu quả**: Tiết kiệm 75% chi phí hạ tầng Bedrock (chỉ $0.006/đơn hàng), độ trễ cực thấp (3-5 giây).

#### Dự Án 2: Multi-Agent Strategic Intelligence Platform (Signal Scout - Giải Nhì)

- **Bối cảnh & Bài toán**: Tự động hóa việc thu thập và phân tích các tín hiệu rải rác của đối thủ cạnh tranh (báo cáo tài chính, thông tin cổ đông).
- **Giải pháp**: Hệ thống Multi-Agent cào dữ liệu tự động, tổng hợp chiến lược đối thủ, phân tích dự báo ROI và đề xuất phương án thích ứng.
- **Kiến trúc Kỹ thuật**: Mô hình Supervisor A2A (Agent-to-Agent) trên AWS Bedrock Agent Core, tích hợp Bedrock Guardrails, LangField (chấm điểm chất lượng dữ liệu), DynamoDB, S3, Amplify.
- **Bài học Hackathon**: Ưu tiên bài toán thực tế (Value-first), quản trị xung đột nhóm và xây dựng MVP demo hiệu quả.

#### Dự Án 3: SA Professional AI Native Assistant (Team Plan)

- **Bối cảnh & Bài toán**: Tự động hóa công việc thiết kế hạ tầng Cloud và tính toán chi phí vốn tốn nhiều thời gian của Solution Architect (SA).
- **Giải pháp**: Nhập yêu cầu bằng ngôn ngữ tự nhiên hoặc tài liệu policy, AI Agent tự động vẽ sơ đồ kiến trúc trên Draw.io, tính toán chi phí dịch vụ AWS và sinh code IaC (Terraform) để deploy tự động.
- **Điểm nổi bật**: Tuân thủ các quy chuẩn/template nội bộ của doanh nghiệp, đảm bảo tính nhất quán giữa các lần khởi tạo.

#### Dự Án 4: Sheper - Autonomous Crowded Monitoring (Team 3K)

- **Bối cảnh & Bài toán**: Giám sát và giải tỏa ùn tắc đám đông tại các khu vực công cộng (sân bay, siêu thị, sự kiện).
- **Giải pháp**: Xử lý luồng video real-time từ camera để đếm số lượng người theo từng khu vực (Zone) và tự động điều phối nhân sự giải tỏa.
- **Kiến trúc Kỹ thuật**: Kinesis Video Streams, AWS Fargate cluster chạy YOLOv11 & ByteTrack, DynamoDB, S3 kết hợp Agent tích hợp Bedrock/OpenAI.

#### Dự Án 5: Adaptive Workflow Engine for AML Investigation (Team Six Pillar)

- **Bối cảnh & Bài toán**: Giảm tỷ lệ Cảnh báo giả (False Positive 90-95%) trong phòng chống rửa tiền (AML) tại các ngân hàng/tổ chức tài chính.
- **Giải pháp**: Hệ thống 3 Layer tự động hóa điều tra giao dịch nghi vấn qua 3 Sub-Agents (KYC Check, Money Flow Check, Sanction Check) kết hợp Human-in-the-loop.
- **Kiến trúc Kỹ thuật**: Layer 1 (Kinesis + SageMaker XGBoost), Layer 2 (Step Functions + Bedrock Multi-Agent + OpenSearch Vector KB + Double-check LLM + Guardrails), Layer 3 (Amplify Dashboard + Cognito).
- **Điểm nổi bật**: Đáp ứng tiêu chuẩn Enterprise Trust (KMS, IAM, Secret Manager, GuardDuty, X-Ray) và cơ chế Double-check loại bỏ Hallucination.

### Những Gì Học Được

#### Tư Duy Thiết Kế Kiến Trúc Agentic AI

- **Kiến trúc Multi-Agent (Supervisor - Sub-agents)**: Phân tách nhiệm vụ chuyên biệt cho từng Agent giúp kiểm soát Context Window, giảm thiểu Hallucination và dễ dàng mở rộng tính năng.
- **Human-in-the-loop**: Đối với các lĩnh vực nhạy cảm (Tài chính, AML, Vận hành), AI Agent đóng vai trò hỗ trợ điều tra và đề xuất, quyết định cuối cùng vẫn thuộc về con người.

#### Kỹ Thuật Tích Hợp Dịch Vụ AWS

- Kết nối linh hoạt giữa **Bedrock Agent Core**, **Step Functions**, **Kinesis**, **SageMaker** và các công cụ Web Scraping/Vector Database.
- Áp dụng các tiêu chuẩn bảo mật Enterprise (WAF, KMS, IAM, Guardrails) và giám sát hệ thống (CloudWatch, X-Ray).

#### Kỹ Năng Thực Chiến Hackathon

- **Scope Management**: Kiểm soát phạm vi dự án vừa đủ (MVP) trong 24 giờ, tránh mở rộng tính năng gây trễ deadline hoặc lỗi hệ thống.
- **Teamwork & Conflict Resolution**: Lắng nghe đồng đội, gạt bỏ cái tôi cá nhân để hướng tới mục tiêu hoàn thành sản phẩm chung.

### Ứng Dụng Vào Công Việc

- **Thử nghiệm mô hình Multi-Agent**: Áp dụng pattern Supervisor Agent kết hợp Sub-Agents cho các bài toán xử lý luồng công việc phức tạp trong dự án.
- **Tự động hóa IaC & Cost Estimation**: Xây dựng kịch bản ứng dụng AI hỗ trợ sinh file Terraform và ước tính chi phí hạ tầng AWS.
- **Triển khai Guardrails & Double-Check**: Áp dụng kiểm soát đầu ra (Bedrock Guardrails) để tăng tính tin cậy cho các ứng dụng LLM trong doanh nghiệp.

### Trải nghiệm trong event

Tham gia sự kiện **Agentic AI Buildweek Hackathon** là một hành trình thử thách nhưng vô cùng đáng nhớ:

- **Áp lực và năng lượng bùng nổ**: Trải nghiệm 24 giờ liên tục brainstorm, viết code, sửa lỗi và chuẩn bị pitching dưới áp lực thời gian.
- **Học hỏi từ thực tế**: Cơ hội xem live demo và lắng nghe giải pháp từ các đội thi xuất sắc, tiếp thu những góc nhìn đa chiều từ kỹ thuật đến bài toán kinh doanh.
- **Tinh thần đồng đội & Kết nối**: Thắt chặt tình đồng đội qua những đêm thức trắng cùng làm project và mở rộng mạng lưới kết nối trong cộng đồng AWS.

#### Một số hình ảnh khi tham gia sự kiện
![Hình ảnh trong sự kiện](./../../../static/images/4-EventParticipated/event3.png)