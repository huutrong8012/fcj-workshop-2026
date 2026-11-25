---
title: "Worklog Tuần 3"
date: 2026-09-28
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---

### Mục tiêu tuần 3:

* **Điều phối Workflow với AWS Step Functions:** Tìm hiểu Standard Workflows, trạng thái Choice, Retry/Catch và mẫu bất đồng bộ .waitForTaskToken.
* **Infrastructure as Code (IaC) với AWS SAM:** Làm quen với AWS Serverless Application Model (SAM CLI) để định nghĩa hạ tầng bằng code thay vì thao tác thủ công.
* **Khám phá Dịch vụ AI Chuyên biệt:** Nghiên cứu Amazon Textract (OCR), Amazon Bedrock (Converse API, Knowledge Bases, Guardrails) và Amazon S3 Vectors.
* **Bảo vệ Dữ liệu & Bằng chứng số:** Tìm hiểu SSE-KMS, S3 Versioning và S3 Object Lock (Governance Mode) phục vụ mức độ sẵn sàng điều tra số (Forensic Readiness).

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Nghiên cứu AWS Step Functions Standard Workflows.<br>- Thực hành xây dựng State Machine có nhánh rẽ Choice, xử lý lỗi Retry/Catch và mẫu lambda:invoke.waitForTaskToken kết hợp SendTaskSuccess để tạm dừng chờ phê duyệt. | 28/09/2026 | 28/09/2026 | AWS Step Functions Developer Guide |
| 3 | - Cài đặt và thực hành AWS SAM CLI (sam init, sam build, sam deploy).<br>- Học cách tổ chức template YAML, tham số hóa môi trường và quản lý tài nguyên Serverless tập trung. | 29/09/2026 | 29/09/2026 | AWS SAM Documentation, AWS Cloud & AI Day Watch Party |
| 4 | - Nghiên cứu Amazon Bedrock Converse API, cơ chế ép định dạng đầu ra JSON và Amazon Bedrock Guardrails (lọc nội dung độc hại và tấn công Prompt Attack qua guardContent).<br>- Tìm hiểu Amazon Bedrock Knowledge Bases sử dụng Titan Text Embeddings V2 kết hợp **Amazon S3 Vectors** để tối ưu chi phí so với OpenSearch Serverless. | 30/09/2026 | 30/09/2026 | Amazon Bedrock User Guide, S3 Vectors Documentation |
| 5 | - Thực hành gọi API DetectDocumentText của Amazon Textract để trích xuất văn bản từ hình ảnh.<br>- Nghiên cứu cơ chế lưu trữ an toàn bằng chứng số trên Amazon S3: mã hóa SSE-KMS (Customer Managed Key), Versioning và Object Lock Governance. | 01/10/2026 | 01/10/2026 | Amazon Textract & Amazon S3 Object Lock Docs |
| 6 | - Họp nhóm tổng hợp kiến thức kỹ thuật tuần 3 và đối chiếu với yêu cầu của **PhishGuard AI**.<br>- Phân tích luồng tiếp nhận đa kênh: Web Portal (Presigned POST) và Email Forward (Amazon SES Inbound Receipt Rules). | 02/10/2026 | 02/10/2026 | Amazon SES Developer Guide, Team Technical Sync |

### Kết quả đạt được tuần 3:

* Nắm vững cơ chế tạm dừng và tiếp tục luồng xử lý bằng .waitForTaskToken trong Step Functions – nền tảng cho tính năng Human-in-the-Loop của dự án.
* Biết cách sử dụng AWS SAM để triển khai hạ tầng lặp lại nhất quán và dễ dàng dọn dẹp tài nguyên.
* Xác định được giải pháp lưu trữ vector tiết kiệm chi phí bằng Amazon S3 Vectors cho Knowledge Base thay vì OpenSearch Serverless.
* Hiểu rõ cơ chế bảo vệ tính toàn vẹn bằng chứng gốc (Forensic Readiness) thông qua S3 Versioning, Object Lock Governance và mã hóa SSE-KMS.
