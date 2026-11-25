---
title: "Worklog Tuần 4"
date: 2026-10-05
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---

### Mục tiêu tuần 4:

* **Kiến trúc Biên & Bảo mật Web (Edge Security):** Nghiên cứu Amazon CloudFront, Origin Access Control (OAC) và AWS WAF (Managed Rules & Rate-based Rules).
* **Thiết kế Kiến trúc Tổng thể PhishGuard AI (v1.0):** Phác thảo sơ đồ kiến trúc 11 bước từ khâu tiếp nhận đa kênh đến thực thi mô phỏng và ghi nhật ký kiểm toán.
* **Thiết kế Mô hình Dữ liệu & Phân quyền:** Xây dựng lược đồ 3 bảng DynamoDB, cấu trúc 5 bucket S3 và ma trận phân quyền tối thiểu (Least Privilege IAM) cho 8 hàm Lambda.
* **Trình bày & Nhận phản hồi từ Mentor:** Báo cáo thiết kế kiến trúc v1.0 với Mentor FCAJ để rà soát các lỗ hổng bảo mật và tính khả thi.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Nghiên cứu cấu hình Amazon CloudFront phân phối React SPA từ S3 (qua OAC) và định tuyến /api/* tới API Gateway HTTP API.<br>- Tìm hiểu AWS WAF Managed Rules, Rate-based Rules và cách cấu hình Origin Request Policy để chuyển tiếp header Authorization. | 05/10/2026 | 05/10/2026 | Amazon CloudFront & AWS WAF Developer Guides |
| 3 | - Vẽ sơ đồ kiến trúc hệ thống **PhishGuard AI** trên Draw.io gồm 11 bước xử lý khép kín.<br>- Thiết kế luồng tách biệt vùng đệm (intake), kho bằng chứng gốc (evidence) và vùng dữ liệu đã che PII (sanitized). | 06/10/2026 | 06/10/2026 | AWS Architecture Icons, Draw.io |
| 4 | - Thiết kế chi tiết 3 bảng DynamoDB (PhishGuard Incidents, PhishGuard AuditLogs, PhishGuard Blocklist) với khóa chính, GSI (ReporterIndex, StatusIndex) và cơ chế Append-only cho AuditLogs.<br>- Xác định vòng đời trạng thái sự cố từ UPLOAD PENDING đến các trạng thái kết thúc. | 07/10/2026 | 07/10/2026 | Amazon DynamoDB Best Practices |
| 5 | - Xây dựng ma trận phân quyền IAM Least Privilege cho 8 hàm Lambda (Reporter API, Admin API, Ingest, Extract & Redact, Triage, Synthesize, PrepareApproval, Containment) và RBAC 3 vai trò trên Cognito (REPORTER, SECOPS ADMIN, AUDITOR). | 08/10/2026 | 08/10/2026 | AWS Well-Architected Security Pillar |
| 6 | - Thuyết trình thiết kế kiến trúc **PhishGuard AI v1.0** với Mentor FCAJ.<br>- Ghi nhận các góp ý phản biện quan trọng về: xử lý chống trùng lặp (Idempotency), xác thực email chuyển tiếp qua SES, và giới hạn khi upload trực tiếp vào S3 qua Presigned POST. | 09/10/2026 | 09/10/2026 | FCAJ Architecture Review Session |

### Kết quả đạt được tuần 4:

* Hoàn thành sơ đồ kiến trúc tổng thể v1.0 cho hệ thống **PhishGuard AI** bao phủ toàn bộ 11 bước xử lý serverless.
* Thiết kế hoàn chỉnh mô hình lưu trữ dữ liệu đa tầng (5 S3 buckets + 1 S3 Vector bucket + 3 bảng DynamoDB) đảm bảo cách ly dữ liệu nhạy cảm.
* Xây dựng được ma trận phân quyền RBAC (3 nhóm người dùng) và IAM Least Privilege (8 Lambda) ngăn chặn tuyệt đối việc LLM hoặc người dùng không có thẩm quyền truy cập bằng chứng thô hay tự ý thực thi hành động chặn.
* Tiếp thu các phản biện chuyên sâu từ Mentor để nâng cấp tài liệu đặc tả kỹ thuật.
