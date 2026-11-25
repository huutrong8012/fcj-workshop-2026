---
title: "Worklog Tuần 2"
date: 2026-09-21
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---

### Mục tiêu tuần 2:

* **Nền tảng Bảo mật & Mạng (IAM & Networking):** Nắm vững nguyên tắc đặc quyền tối thiểu (Least Privilege) trên IAM và thiết kế hạ tầng mạng VPC an toàn.
* **Kiến trúc Serverless & Event-Driven:** Tìm hiểu sâu mô hình hướng sự kiện với AWS Lambda, Amazon S3 Events, Amazon DynamoDB và API Gateway.
* **Khởi động Đề tài Nhóm (Ideation):** Phân tích bài toán lừa đảo trực tuyến (Phishing) đa kênh tại doanh nghiệp Việt Nam và đề xuất ý tưởng **PhishGuard AI**.
* **Thiết kế Sơ bộ & Phản biện:** Xác định phạm vi bài toán, lý do kết hợp luật tất định (Deterministic Policy Engine) với AI (Human-in-the-Loop SOAR).

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Thực hành nâng cao về AWS IAM: IAM Roles, Permission Policies, Trust Policies và IAM Policy Simulator.<br>- Nghiên cứu nguyên tắc Least Privilege và phân tách vai trò (Separation of Duties) trong môi trường đa dịch vụ. | 21/09/2026 | 21/09/2026 | AWS IAM User Guide, AWS Security Best Practices |
| 3 | - Tìm hiểu sâu mô hình Serverless & Event-Driven Architecture.<br>- Thực hành kết nối Amazon S3 Event Notifications kích hoạt AWS Lambda và ghi dữ liệu vào Amazon DynamoDB (On-Demand, GSI, TTL). | 22/09/2026 | 22/09/2026 | AWS Serverless Workshop, DynamoDB Developer Guide |
| 4 | - Nghiên cứu Amazon API Gateway (HTTP API) và cơ chế xác thực JWT thông qua Amazon Cognito User Pools.<br>- Thực hành cấu hình Cognito User Groups để phân quyền theo vai trò (RBAC). | 23/09/2026 | 23/09/2026 | Amazon Cognito & API Gateway Documentation |
| 5 | - Họp nhóm nghiên cứu bài toán thực tế: thực trạng lừa đảo qua email giả mạo ngân hàng/đối tác, tin nhắn Zalo/WhatsApp giả danh lãnh đạo yêu cầu chuyển khoản gấp hoặc đánh cắp OTP.<br>- Phân tích rủi ro khi giao toàn quyền chặn cho AI (rò rỉ PII, Prompt Injection, False Positive) và quá tải của đội SecOps khi xử lý thủ công. | 24/09/2026 | 24/09/2026 | Báo cáo An toàn thông tin VN, OWASP Top 10 for LLM |
| 6 | - Chốt tên đề tài: **PhishGuard AI** – Nền tảng phân tích lừa đảo đa kênh và điều phối ứng phó sự cố có người duyệt (Human-in-the-Loop Phishing Triage & SOAR Copilot) trên AWS Serverless.<br>- Viết bản đề xuất dự án (Project Proposal v1.0) và phân công nhiệm vụ nghiên cứu kỹ thuật cho 5 thành viên. | 25/09/2026 | 25/09/2026 | FCAJ Project Proposal Template, Team Brainstorming |

### Kết quả đạt được tuần 2:

* Thành thạo cấu hình phân quyền chi tiết (fine-grained IAM policies) và cơ chế xác thực JWT với Cognito User Pools.
* Nắm vững thiết kế bảng dữ liệu NoSQL trên DynamoDB với Global Secondary Index (GSI) và cơ chế TTL tự động dọn dẹp dữ liệu tạm.
* Xác định rõ bài toán trọng tâm và giá trị cốt lõi của dự án **PhishGuard AI**: kết hợp chấm điểm rủi ro tất định (minh bạch, bất biến trước Prompt Injection) với Amazon Bedrock để giải thích và truy xuất quy trình ứng phó (RAG), kèm cơ chế phê duyệt của con người (Human-in-the-Loop).
* Hoàn thành tài liệu Project Proposal v1.0 sẵn sàng trình bày với Mentor FCAJ.
