---
title: "Worklog Tuần 11"
date: 2026-11-23
weight: 11
chapter: false
pre: " <b> 1.11. </b> "
---

### Mục tiêu tuần 11 (Giai đoạn MVP 3 & Bàn giao Hoàn thiện):

* **Phát triển Web Portal & Dashboard (React SPA) & Kích hoạt AWS WAF:** Xây dựng giao diện người dùng, tích hợp xác thực Cognito JWT, triển khai qua S3 + CloudFront (OAC) và bật AWS WAF (EnableWaf=true).
* **Tích hợp 3 Kịch bản Demo 1-Click & Thực thi 15 Ca Kiểm thử Bắt buộc:** Kết nối 3 nút demo qua pipeline thực tế và nghiệm thu toàn bộ 15 ca kiểm thử chức năng/bảo mật.
* **Giám sát Vận hành & Hoàn thiện Workshop:** Cấu hình CloudWatch Alarms/Dashboard, AWS Budgets, đối chiếu sơ đồ kiến trúc khớp 100% thực tế và hoàn thiện tài liệu Workshop 7 chương.
* **Quay Video Demo, Diễn tập & Nộp Sản phẩm:** Quay video trình diễn toàn bộ hệ thống, diễn tập thuyết trình nhóm và bàn giao hồ sơ lên FCAJ Portal.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Xây dựng và triển khai React **Reporter Portal** cùng **SecOps & Auditor Dashboard** lên S3 web qua CloudFront (OAC), kiểm tra chuyển tiếp header Authorization và kích hoạt **AWS WAF** (Managed Rules + Rate-based rule). | 23/11/2026 | 23/11/2026 | PhishGuard Spec 1.1, 3.2, 5.1 |
| 3 | - Chạy nghiệm thu **3 kịch bản demo 1-click** trên hệ thống deploy thật:<br>  1. Ảnh Zalo giả Giám đốc → 65, HIGH, đề xuất FINANCE VERIFICATION ALERT.<br>  2. Email giả Techcombank → 80, CRITICAL, đề xuất BLOCK DOMAIN (DRY RUN).<br>  3. Email có bẫy Prompt Injection → điểm giữ nguyên 80, CRITICAL, gắn cờ PROMPT INJECTION DETECTED. | 24/11/2026 | 24/11/2026 | PhishGuard Spec 8.1 |
| 4 | - Thực thi và ghi nhận bằng chứng cho toàn bộ **15 ca kiểm thử bắt buộc** (mục 8.2): trùng lặp key, 409 conflict, retry Ingest không tạo version evidence thừa, SES từ người gửi lạ (UNVERIFIED SENDER), thông báo ngân hàng an toàn không bị cộng điểm nhầm, ảnh mờ (NEEDS MORE INFO), timeout 60s (ESCALATED TIMEOUT), kiểm tra phân quyền 403/404. | 25/11/2026 | 25/11/2026 | PhishGuard Spec 8.2 |
| 5 | - Cấu hình **CloudWatch Alarms** (SQS DLQ ≥ 1 message, Step Functions FAILED, lỗi Lambda, throttle Bedrock), **CloudWatch Dashboard** và **AWS Budgets**.<br>- Đối chiếu sơ đồ PhishGuard Architecture v2.2.drawio khớp 100% hạ tầng thực tế; hoàn thiện tài liệu **Workshop** (đủ 7 phần chuẩn FCAJ, bảng chi phí Pricing Calculator và hướng dẫn Clean-up). | 26/11/2026 | 26/11/2026 | PhishGuard Spec 9, 10 |
| 6 | - Quay và biên tập **Video Demo sản phẩm** (Full HD có thuyết minh), tổ chức diễn tập thuyết trình nhóm 5 thành viên và nộp toàn bộ hồ sơ (Link Production, Video Demo, Workshop, Sơ đồ Kiến trúc, Mã nguồn) lên FCAJ Portal. | 27/11/2026 | 27/11/2026 | FCAJ Portal, AWS Billing Console |

### Kết quả đạt được tuần 11:

* Hoàn thiện 100% hệ thống **PhishGuard AI** trên môi trường Production với đầy đủ lớp bảo vệ AWS WAF, CloudFront (OAC), Cognito RBAC, Step Functions, Amazon Bedrock (RAG + Guardrails) và S3 Object Lock.
* Cả 3 kịch bản demo 1-click và toàn bộ 15 ca kiểm thử bắt buộc đều vượt qua (PASS) trên môi trường AWS thực tế.
* Hệ thống giám sát vận hành và cảnh báo ngân sách (CloudWatch Alarms, Dashboard, AWS Budgets) hoạt động ổn định.
* Hoàn thiện tài liệu **Workshop** 7 chương, quay xong Video Demo và nộp đầy đủ sản phẩm lên FCAJ Portal.
