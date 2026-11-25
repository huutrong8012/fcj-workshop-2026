---
title: "Đánh giá Well-Architected"
date: 2026-11-24
weight: 6
chapter: false
pre: " <b> 5.6. </b> "
---

Chọn một sự cố demo và mở đồng thời ba màn hình: trang chi tiết sự cố trên web, lịch sử chạy của Step Functions và phiên bản object gốc trong bucket S3 evidence. Ba hiện vật này giúp đối chiếu thiết kế theo **AWS Well-Architected Framework**:

| Trụ cột | Quyết định thiết kế trong PhishGuard AI | Cách kiểm tra và giới hạn đánh giá |
| :--- | :--- | :--- |
| **Sơ đồ và triển khai** | CloudFront gắn WAF; đường web và SES hội tụ tại Ingest; lưu bằng chứng gốc trước khi mở workflow; Step Functions giữ callback chờ người duyệt. | Đối chiếu CloudFront behaviors, SES receipt rule, template.yaml và execution trace với mục 5.1. Lambda dùng mạng do AWS quản lý. |
| **Operational Excellence** | Change set cho phép xem trước cập nhật hạ tầng; hai CloudWatch alarm cảnh báo qua SNS khi workflow lỗi hoặc DLQ có tin nhắn. | Kiểm tra trạng thái alarm, ARN đích SNS và truy vết request ID trong CloudWatch Logs. |
| **Security** | Cognito JWT và RBAC kiểm soát API; S3 evidence bật SSE-KMS, Versioning và Object Lock Governance 7 ngày; che PII trước khi gọi AI; phê duyệt qua Admin API. | Chạy H02 và H15 với hai reporter và auditor; kiểm thử chặn header tại WAF (403); ràng buộc presigned POST và magic bytes cho upload trực tiếp lên S3. |
| **Reliability** | Ingest chống trùng bằng source_intake_key; lỗi bất đồng bộ chuyển vào SQS DLQ; Step Functions có nhánh timeout 48 giờ; Synthesize có fallback tất định. | Chạy H01/H04 và H13/H14 trên môi trường demo. Triển khai trên một Region (us-east-1). |
| **Performance Efficiency** | Upload thẳng lên S3 giảm tải cho API Gateway; xử lý ảnh OCR/QR dùng Lambda x86_64 512 MiB; .waitForTaskToken không tốn chi phí compute trong lúc chờ duyệt. | Theo dõi Duration, Billed Duration và Max Memory Used của Lambda giữa nhóm văn bản và nhóm ảnh. |
| **Cost Optimization** | Tính phí theo lượt dùng thực tế (Lambda, Step Functions, DynamoDB On-Demand, S3 Vectors); phần cố định chủ yếu nằm ở AWS WAF (~75%). | Đối chiếu bảng tính mục 5.7 với dữ liệu xuất từ AWS Cost Explorer và cảnh báo Budgets. |
| **Sustainability** *(Ghi nhận)* | Xử lý hướng sự kiện không duy trì máy chủ nhàn rỗi; vòng đời S3 tự xóa dữ liệu tạm ở intake (uploads/, inbound/) sau 3 ngày. | Kiểm tra lifecycle rule và phân biệt rõ dữ liệu tạm với bằng chứng đang trong thời hạn bảo toàn 7 ngày. |
