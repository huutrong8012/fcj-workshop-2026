---
title: "Thu dọn tài nguyên"
date: 2026-11-24
weight: 8
chapter: false
pre: " <b> 5.8. </b> "
---

Thực hiện dọn dẹp tài nguyên theo đúng thứ tự phụ thuộc sau khi hoàn tất buổi đánh giá của hội đồng:

1. **Duy trì môi trường đến khi chấm xong:** Giữ đường dẫn production hoạt động nếu giám khảo còn cần kiểm tra trực tiếp. Xuất toàn bộ minh chứng kiểm thử, bản dự toán chi phí và báo cáo Billing trước khi xóa.
2. **Dừng tiếp nhận mới & Gỡ rule SES:** Ngừng gửi báo cáo mới, rà soát các execution đang ở trạng thái PENDING_APPROVAL trên Step Functions và gỡ riêng receipt rule của lab khỏi Amazon SES (giữ nguyên bản ghi MX nếu dùng chung tên miền).
3. **Tháo liên kết & Xóa CloudFront, WAF:** Gỡ liên kết Web ACL PhishGuardEdge khỏi CloudFront distribution, chọn **Disable** distribution, đợi trạng thái Deployed rồi xóa. Sau đó xóa stack phishguard-edge-waf và hủy đăng ký email demo trên SNS.
4. **Xóa Bedrock Knowledge Base & S3 Vectors:** Xóa data source và Knowledge Base trên Amazon Bedrock trước, sau đó xóa stack phishguard-kb-prereqs cùng S3 Vector index/bucket.
5. **Làm rỗng S3 & Xóa Stack SAM chính:** Xóa toàn bộ object trong các bucket không khóa (web, intake, sanitized, kb-docs). Với bucket evidence bật Object Lock Governance, chờ hết hạn 7 ngày hoặc dùng vai trò có quyền bypass retention để xóa toàn bộ version và delete marker trước khi xóa stack.
6. **Lên lịch xóa khóa KMS & Kiểm tra lại Billing:** Xóa stack phishguard-ai trên CloudFormation, kiểm tra lại CloudWatch logs/alarms, SQS, Cognito domain, Budgets; đặt lịch xóa khóa KMS Customer Managed Key (thời gian chờ 7–30 ngày) và kiểm tra lại **AWS Cost Explorer** sau 24 giờ.
