---
title: "Worklog Tuần 9"
date: 2026-11-11
weight: 9
chapter: false
pre: " <b> 1.9. </b> "
---

### Mục tiêu tuần 9 (Giai đoạn MVP 1):

* **Triển khai Hạ tầng Nền tảng (SAM Deploy):** Đưa toàn bộ S3, DynamoDB, Cognito, Ingest Lambda và Step Functions khung lên môi trường AWS us-east-1.
* **Kiểm thử Sớm Các Rủi ro Kỹ thuật Lớn (Early Risk Spike):** Thực nghiệm ngay trong tuần đầu triển khai MVP đối với OCR tiếng Việt, chất lượng sinh tiếng Việt của Nova Lite, S3 Object Lock If-None-Match, Presigned POST CORS và luồng SES Inbound thật.
* **Khởi chạy Luồng Xử lý Văn bản End-to-End:** Đảm bảo báo cáo dạng văn bản đi qua Reporter API → intake → Ingest → evidence → Step Functions thành công.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Deploy khung hạ tầng AWS SAM đầu tiên lên us-east-1 (S3 buckets, KMS CMK, DynamoDB tables, Cognito, SQS DLQ).<br>- Hoàn tất xác minh tên miền SES, cấu hình SES Receipt Rule (S3 action ghi vào inbound/ rồi Lambda action gọi Ingest). | 11/11/2026 | 11/11/2026 | PhishGuard Spec Section 10 |
| 3 | - **Cổng quyết định OCR:** Chạy thực nghiệm Amazon Textract DetectDocumentText trên 5 ảnh chụp màn hình Zalo tiếng Việt.<br>- Đánh giá độ chính xác nhận diện chuỗi số (SĐT, STK, CCCD) và từ khóa bỏ dấu ("chuyen khoan", "OTP", "ma xac nhan") để chốt sử dụng Textract. | 12/11/2026 | 12/11/2026 | PhishGuard Spec 7.2, Amazon Textract |
| 4 | - **Kiểm thử rủi ro lưu trữ & Upload:** Xác nhận hành vi ghi chống trùng PutObject với If-None-Match: * trên bucket evidence đã bật Versioning + Object Lock Governance.<br>- Kiểm thử luồng cấp Presigned POST từ Reporter API và upload file từ trình duyệt kèm cấu hình S3 CORS. | 13/11/2026 | 13/11/2026 | Amazon S3 Conditional Writes Docs |
| 5 | - **Kiểm thử rủi ro LLM tiếng Việt:** Chạy thử **Amazon Nova Lite** qua Converse API với 3 mẫu kịch bản demo để đánh giá khả năng tóm tắt tiếng Việt tự nhiên, tuân thủ JSON schema và sinh 3–4 khuyến nghị chính xác. | 14/11/2026 | 14/11/2026 | PhishGuard Spec 7.6, Amazon Bedrock |
| 6 | - Lập trình hoàn chỉnh Reporter API (POST /api/reports, POST /api/uploads/presign) và Ingest Lambda (kiểm tra magic bytes, tính SHA-256, chống trùng source intake key, khởi chạy Step Functions).<br>- Kiểm thử thực tế gửi email forward thật tới soc@domain và xác nhận dmarcVerdict == PASS map đúng reporter id. | 15/11/2026 | 15/11/2026 | PhishGuard Spec 2, 3, 5.3 |

### Kết quả đạt được tuần 9:

* Triển khai thành công hạ tầng nền tảng **PhishGuard AI** bằng AWS SAM lên us-east-1.
* Xác nhận toàn bộ 5 điểm rủi ro kỹ thuật lớn đều đạt tiêu chí:
  * Textract đọc chính xác các chuỗi số và từ khóa tiếng Việt sau chuẩn hóa bỏ dấu trên ảnh Zalo.
  * Amazon Nova Lite sinh phản hồi JSON chuẩn schema với văn phong tiếng Việt tự nhiên.
  * If-None-Match: * hoạt động chính xác trên bucket bật Versioning + Object Lock Governance, ngăn tạo version rác khi retry.
  * Luồng SES Inbound nhận email thật, xác minh dmarcVerdict == PASS và kích hoạt Ingest thành công.
* Luồng tiếp nhận báo cáo văn bản đã chạy thông suốt qua Ingest và kích hoạt Step Functions.
