---
title: "Worklog Tuần 8"
date: 2026-11-02
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---

### Mục tiêu tuần 8:

* **Xây dựng Thư viện Lõi Độc lập (Core Logic Prototyping):** Phát triển và kiểm thử đơn vị (Unit Test) cho các module xử lý thuần túy trước khi gắn lên AWS: Che PII, Defang URL và Policy Engine.
* **Kiểm thử Thuật toán Nhận diện Tên miền Giả mạo:** Đảm bảo độ chính xác của BRAND LOOKALIKE DOMAIN với Public Suffix List và danh sách thương hiệu Việt Nam.
* **Phát triển Khung Hạ tầng AWS SAM Base:** Viết template SAM khởi tạo các tài nguyên lưu trữ (S3, DynamoDB), định danh (Cognito) và hàng đợi lỗi (SQS DLQ).
* **Tạo Tài khoản Kiểm thử Thực tế:** Thiết lập các tài khoản người dùng trên Cognito với email thật (không dùng .local) để phục vụ kiểm thử SES.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Lập trình module **PII Redactor**: nhận diện và thay thế nhất quán CCCD/CMND, SĐT Việt Nam, số tài khoản ngân hàng (kèm từ khóa), mã OTP và phần local email thành các token định danh (BANK ACCOUNT 1, EMAIL USER 1).<br>- Viết unit test kiểm tra tính bảo toàn quan hệ thực thể. | 02/11/2026 | 02/11/2026 | Python Regex & NLP Sanitization Patterns |
| 3 | - Lập trình module **URL Extractor & Defanger**: trích xuất URL an toàn (tuyệt đối không active fetch), chuẩn hóa canonical value và tạo display value (hxxp, [.]).<br>- Tích hợp Public Suffix List nội bộ và viết thuật toán BRAND LOOKALIKE DOMAIN (tách nhãn chấm và gạch nối, chuẩn hóa homoglyph 0→o, 1→l, rn→m). | 03/11/2026 | 03/11/2026 | Public Suffix List, PhishGuard Spec 7.4 |
| 4 | - Lập trình **Deterministic Policy Engine**: cài đặt 6 luật chấm điểm, cơ chế kiểm tra câu cảnh báo an toàn của ngân hàng để tránh báo nhầm, bộ phát hiện mẫu Prompt Injection và logic sinh proposed actions (BLOCK DOMAIN, FINANCE VERIFICATION ALERT) hoặc gắn cờ requires manual review. | 04/11/2026 | 04/11/2026 | PhishGuard Spec 7.5 |
| 5 | - Viết template template.yaml trong AWS SAM định nghĩa 5 bucket S3 (web, intake, evidence bật Object Lock Governance 7 ngày + SSE-KMS, sanitized, kb-docs), 3 bảng DynamoDB (bật PITR) và SQS DLQ (retention 2 ngày). | 05/11/2026 | 05/11/2026 | AWS SAM Resource Reference |
| 6 | - Cấu hình Amazon Cognito User Pool, App Client và 3 nhóm RBAC (REPORTER, SECOPS ADMIN, AUDITOR) trong SAM template.<br>- Khởi tạo các tài khoản demo dùng địa chỉ email thật có khả năng gửi/nhận thư để chuẩn bị test luồng SES Inbound. | 06/11/2026 | 06/11/2026 | Amazon Cognito Developer Guide |

### Kết quả đạt được tuần 8:

* Hoàn thành 100% các thư viện xử lý lõi (PII Redactor, URL/Domain Analyzer, Deterministic Policy Engine) đạt tỷ lệ vượt qua toàn bộ unit test cục bộ.
* Sẵn sàng bộ khung hạ tầng AWS SAM cho tầng lưu trữ, bảo mật khóa KMS, hàng đợi DLQ và hệ thống định danh Cognito.
* Chuẩn bị đầy đủ tài khoản kiểm thử thực tế cho 3 vai trò người dùng.
