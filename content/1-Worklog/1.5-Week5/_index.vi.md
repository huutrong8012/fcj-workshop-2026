---
title: "Worklog Tuần 5"
date: 2026-10-12
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---

### Mục tiêu tuần 5:

* **Hoàn thiện Đặc tả Kỹ thuật (v2.0 → v2.1):** Cụ thể hóa các thuật toán phân tích lừa đảo tất định (Policy Engine), trích xuất/defang URL, phát hiện tên miền giả mạo và quy tắc che PII.
* **Ánh xạ AWS Well-Architected Framework:** Đánh giá kiến trúc dự án theo 6 trụ cột của AWS Well-Architected (đặc biệt tập trung vào Security, Reliability và Cost Optimization).
* **Xây dựng Bộ dữ liệu Tri thức (RAG Knowledge Base):** Biên soạn tài liệu quy trình ứng phó sự cố SecOps, quy định xác thực chuyển khoản và kịch bản lừa đảo tại Việt Nam.
* **Thiết kế Bộ Kịch bản Demo & Kiểm thử:** Xây dựng 3 kịch bản demo 1-click và 15 ca kiểm thử chức năng/bảo mật bắt buộc.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Đặc tả chi tiết bộ luật chấm điểm rủi ro tất định (KNOWN BLOCKED IOC +50, BRAND LOOKALIKE DOMAIN +40, FINANCIAL OR OTP REQUEST +30, EXEC IMPERSONATION +25, URGENCY PRESSURE +10, HEADER ANOMALY +10).<br>- Thiết kế cơ chế gắn cờ PROMPT INJECTION DETECTED không cộng điểm để chứng minh tính bất biến của điểm rủi ro. | 12/10/2026 | 12/10/2026 | MITRE ATT&CK, PhishGuard Technical Spec |
| 3 | - Thiết kế quy tắc che PII có cấu trúc (CCCD/CMND, SĐT Việt Nam, số tài khoản ngân hàng, mã OTP, phần local email) nhưng giữ lại quan hệ thực thể (BANK ACCOUNT 1, EMAIL USER 1) để AI vẫn hiểu ngữ cảnh mà không lộ dữ liệu gốc.<br>- Xây dựng quy tắc chuẩn hóa IOC (canonical vs. defang hxxps://...[.]com). | 13/10/2026 | 13/10/2026 | NIST PII Confidentiality Guide |
| 4 | - Biên soạn bộ tài liệu chuẩn cho Bedrock Knowledge Base (kb-docs): Sổ tay SecOps, Quy định xác minh giao dịch tài chính qua điện thoại, và Danh mục nhận diện lừa đảo phổ biến tại Việt Nam.<br>- Xây dựng dữ liệu mẫu nạp sẵn (SEED) cho bảng PhishGuard Blocklist. | 14/10/2026 | 14/10/2026 | Tài liệu nghiệp vụ SOC & Ngân hàng |
| 5 | - Đối chiếu kiến trúc với AWS Well-Architected Framework (Operational Excellence, Security, Reliability, Performance Efficiency, Cost Optimization, Sustainability).<br>- Lập bảng tính ước tính chi phí trên AWS Pricing Calculator với giả định 300 sự cố/tháng và so sánh chi phí giữa S3 Vectors với OpenSearch Serverless. | 15/10/2026 | 15/10/2026 | AWS Well-Architected Framework, AWS Pricing Calculator |
| 6 | - Thiết kế 3 kịch bản demo 1-click (Ảnh Zalo giả Giám đốc, Email giả ngân hàng Techcombank, Email chứa bẫy Prompt Injection) và danh sách 15 ca kiểm thử nghiệm thu hệ thống.<br>- Tổng hợp tài liệu Đặc tả Kỹ thuật v2.1. | 16/10/2026 | 16/10/2026 | Team Review & QA Planning |

### Kết quả đạt được tuần 5:

* Hoàn thiện bộ quy tắc chấm điểm tất định minh bạch (0–100 điểm) và cơ chế che PII bảo toàn quan hệ thực thể.
* Biên soạn hoàn chỉnh kho tri thức chuyên ngành (kb-docs) phục vụ tính năng RAG trên Amazon Bedrock.
* Hoàn tất bảng phân tích AWS Well-Architected và dự toán chi phí vận hành trên AWS Pricing Calculator, chứng minh hiệu quả tiết kiệm chi phí nhờ sử dụng S3 Vectors, DynamoDB On-Demand và lifecycle tự động xóa vùng đệm intake.
* Chốt bộ 3 kịch bản demo 1-click và 15 ca kiểm thử bắt buộc làm thước đo chất lượng cho giai đoạn triển khai.
