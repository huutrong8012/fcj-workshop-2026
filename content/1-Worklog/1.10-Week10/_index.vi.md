---
title: "Worklog Tuần 10"
date: 2026-11-16
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---

### Mục tiêu tuần 10 (Giai đoạn MVP 2):

* **Hoàn thiện Chuỗi Phân tích Lõi (Extract → Triage → Synthesize):** Triển khai các hàm Lambda Extract & Redact, Triage và Synthesize tích hợp Bedrock Knowledge Base + Guardrails.
* **Triển khai Luồng Phê duyệt & Thực thi (Human-in-the-Loop):** Xây dựng PrepareApproval, Admin API, Containment Lambda và nhánh NEEDS REVIEW.
* **Đảm bảo Tính Toàn vẹn & Chống Bấm Đúp:** Hoàn thiện logic xử lý retry SendTaskSuccess, timeout phê duyệt (ESCALATED TIMEOUT) và ghi nhật ký kiểm toán không thể sửa/xóa (AuditLogs).
* **Bắt đầu Viết Tài liệu Workshop Song song:** Chụp ảnh màn hình từng bước cấu hình dịch vụ và lưu trữ phục vụ bài nộp Workshop.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Triển khai Extract & Redact Lambda: phân tích file .eml (tách phần message/rfc822 khi là thư forward), gọi Textract cho ảnh, kiểm tra chất lượng OCR (NEEDS MORE INFO nếu mờ/quá ngắn), che PII có cấu trúc và lưu sang bucket sanitized. | 16/11/2026 | 16/11/2026 | PhishGuard Spec 6, 7.1, 7.3 |
| 3 | - Triển khai Triage Lambda: tích hợp Policy Engine tất định, defang URL, đối chiếu PhishGuard Blocklist (chỉ tính điểm với SEED và APPROVED CONFIRMED), tính điểm rủi ro và sinh proposed actions. | 17/11/2026 | 17/11/2026 | PhishGuard Spec 7.4, 7.5 |
| 4 | - Khởi tạo **Amazon Bedrock Knowledge Base** kết nối S3 kb-docs và **Amazon S3 Vectors**, đồng bộ dữ liệu (Ingestion Job) và cấu hình **Bedrock Guardrail**.<br>- Triển khai Synthesize Lambda: truy xuất Top-3 RAG, gọi Converse API đặt bằng chứng trong guardContent, và cài đặt cơ chế FALLBACK tất định khi lỗi hoặc bị Guardrails chặn. | 18/11/2026 | 18/11/2026 | PhishGuard Spec 7.6, Bedrock Knowledge Base Docs |
| 5 | - Triển khai PrepareApproval Lambda (lưu sfn task token trước rồi mới gửi SNS), nhánh NotifyManualReview cho các ca NEEDS REVIEW, và Containment Lambda (cập nhật Blocklist ở chế độ DRY RUN / INTERNAL LIST và ghi AuditLogs). | 19/11/2026 | 19/11/2026 | PhishGuard Spec 6, 4.3 |
| 6 | - Triển khai Admin API (GET /api/incidents, GET /api/incidents/{id}/raw-evidence-url cấp Presigned GET 60s có ghi AuditLog, POST /api/incidents/{id}/decision chống bấm đúp và xử lý retry SendTaskSuccess).<br>- Bắt đầu soạn thảo khung tài liệu **AWS Workshop** và chụp hình minh họa kiến trúc backend. | 20/11/2026 | 20/11/2026 | PhishGuard Spec 5.2, 6, 10 |

### Kết quả đạt được tuần 10:

* Hoàn thành 100% pipeline xử lý backend của **PhishGuard AI** từ khâu trích xuất bằng chứng đến điều phối phê duyệt và ghi nhật ký kiểm toán.
* Xác minh thành công cơ chế **Guardrails + Fallback**: khi nội dung chứa Prompt Injection hoặc bị chặn, hệ thống vẫn bảo toàn điểm rủi ro tất định và trả về bản tóm tắt an toàn.
* Triển khai chặt chẽ quy trình phê duyệt Admin API chịu lỗi mạng và chống bấm đúp (Idempotent Decision).
* Khởi tạo bản nháp tài liệu Workshop cùng bộ ảnh chụp cấu hình thực tế trên AWS Console.
