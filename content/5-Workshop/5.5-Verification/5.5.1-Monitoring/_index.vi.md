---
title: "Tài nguyên và giám sát"
date: 2026-11-24
weight: 1
chapter: false
pre: " <b> 5.5.1. </b> "
---

### Xuất bản đối chiếu cấu hình tự động

Chạy công cụ tools/audit_phishguard.py trong thư mục lab/ để kiểm kê cấu hình đã triển khai (8 Lambda, 5 bucket S3, 3 bảng DynamoDB, 2 CloudWatch alarm, S3 Object Lock Governance 7 ngày và layer OCR 512 MiB x86_64):

```powershell
$recipient = Read-Host 'SES lab recipient'
.\.venv\Scripts\python.exe ..	oolsudit_phishguard.py --profile $pgProfile --region us-east-1 --stack phishguard-ai --recipient $recipient --output .\phishguard-account.json
```

### Danh mục đối chiếu thủ công trên AWS Console

| Dịch vụ trên AWS Console | Nội dung cần đối chiếu | Bằng chứng cần lưu giữ |
| :--- | :--- | :--- |
| **CloudFormation (phishguard-ai)** | Account/region, tên bucket, HTTP API, URL dashboard, KB ID, amazon.nova-lite-v1:0, ContainmentMode=DRY_RUN, timeout 172800 giây. | File JSON đối chiếu và ảnh chụp tham số stack. |
| **CloudFront (General / Security / Behaviors)** | Domain khớp URL dashboard; Web ACL PhishGuardEdge (scope CloudFront) đã gắn kèm managed rules và rate-based rule. | Ảnh chụp tab Security thể hiện liên kết Web ACL. |
| **Amazon S3 (web, intake, evidence)** | Block Public Access, OAC, CORS trên intake, S3 event chỉ tại uploads/, lifecycle 3 ngày, evidence bật Versioning + Governance 7 ngày + SSE-KMS. | Ảnh Properties của bucket và đối chiếu raw_s3_version_id. |
| **SES, SNS, Cognito, Bedrock** | Receipt rule active; SNS email subscription Confirmed; Cognito app client & 3 nhóm quyền; KB active và phiên bản Guardrail. | Mã incident SES, kết quả truy xuất playbook và trạng thái Guardrail. |
| **CloudWatch Alarms & Metrics** | Hai alarm PhishGuard-Workflow-Failed và PhishGuard-Ingest-DLQ-NotEmpty ở trạng thái OK và bật action trỏ về SNS SecOps. | Ảnh chụp hai alarm và metric UnverifiedSender. |
| **Billing → Budgets** | Cost budget theo tháng với ngưỡng cảnh báo thực tế (80%) và dự báo (100%) gửi qua email. | Tên budget, ngưỡng cảnh báo và trạng thái notification. |

![Trạng thái hai CloudWatch Alarm](/images/5-Workshop/5.5-Verification/5.5.1-Monitoring/cw-alarms.png)
