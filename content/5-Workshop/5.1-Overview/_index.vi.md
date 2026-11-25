---
title: "Bài toán và kiến trúc"
date: 2026-11-24
weight: 1
chapter: false
pre: " <b> 5.1. </b> "
---

### Bối cảnh bài toán

Một nhân viên chuyển tiếp thư nghi lừa đảo, còn đồng nghiệp gửi ảnh chụp màn hình cùng chiến dịch. SecOps cần giữ bản gốc của từng báo cáo, kiểm soát ai được xem dữ liệu nhạy cảm và truy lại lý do xử lý. Workshop này dựng quy trình xử lý tại **US East (N. Virginia) (us-east-1)**.

![Kiến trúc hệ thống PhishGuard AI](/images/5-Workshop/5.1-Overview/architecture.png)

### Cách đọc sơ đồ kiến trúc

1. **Truy cập biên và Portal:** Trình duyệt tải ứng dụng React qua **Amazon CloudFront** (có gắn **AWS WAF**), đăng nhập **Amazon Cognito** và gửi JWT đến **API Gateway HTTP API**. Reporter API và Admin API đọc/ghi **Amazon DynamoDB** để tra cứu hồ sơ, kiểm tra quyền và ghi nhận quyết định.
2. **Tiếp nhận báo cáo đa kênh:** Với ảnh hoặc file .eml, trình duyệt nhận form ký sẵn rồi POST trực tiếp đến **Amazon S3 (intake)**. Với văn bản dán vào và mẫu demo, Reporter API ghi hoặc sao chép file vào uploads/{uuid}/raw, kích hoạt sự kiện S3 gọi Ingest. Email chuyển tiếp đi đường riêng: **Amazon SES** lưu MIME gốc vào inbound/ rồi gọi bất đồng bộ Ingest kèm thông tin xác thực lần nhận.
3. **Bảo toàn bằng chứng và mở Workflow:** Ingest kiểm tra định dạng, tính SHA-256 và ghi bản gốc vào **bucket evidence** (bật Versioning, SSE-KMS và Object Lock Governance 7 ngày). Sau khi lưu thành công, Ingest mở execution **AWS Step Functions** mang tên incident_id, truyền tham chiếu raw_s3_key, raw_s3_version_id và evidence_sha256. Bucket sanitized lưu riêng bản phân tích đã che PII; các lượt gọi Ingest lỗi sau retry được đẩy vào **SQS DLQ** có cảnh báo **CloudWatch**.
4. **Trích xuất, Chấm điểm tất định & Tổng hợp AI:** Extract & Redact đọc bản gốc, dùng Tesseract tiếng Việt/Anh và ZXing-C++ giải mã QR cho ảnh rồi che PII. Ảnh thiếu cả nội dung OCR lẫn URL QR hợp lệ đi tới NEEDS_MORE_INFO. Triage chấm điểm theo luật và đối chiếu IOC nội bộ. Synthesize truy xuất playbook từ **Amazon Bedrock Knowledge Bases** (**S3 Vectors**) và gọi **Amazon Nova Lite** kèm **Bedrock Guardrails**; khi tổng hợp lỗi, trang sự cố hiện FALLBACK cùng kết quả theo luật.
5. **Điều hướng và Cổng chờ phê duyệt:** Nếu thiếu hành động phù hợp, ca HIGH/CRITICAL chuyển NEEDS_REVIEW và báo **Amazon SNS**, mức thấp hơn thành NO_ACTION. Khi có đề xuất hành động, PrepareApproval ghi task token và trạng thái PENDING_APPROVAL trước khi gửi SNS, giữ Step Functions tạm dừng tối đa 48 giờ.
6. **Quyết định của Admin và Thực thi mô phỏng:** Quản trị viên quyết định trên web. Admin API kiểm tra quyền, kiểm tra trạng thái rồi gửi SendTaskSuccess mang kết quả về workflow. Từ chối thành REJECTED, quá hạn thành ESCALATED_TIMEOUT. Duyệt ở chế độ DRY_RUN ghi kết quả mô phỏng và nhật ký kiểm toán.

### Hai ranh giới kỹ thuật cần lưu ý trên sơ đồ

- **Biên WAF và Upload trực tiếp:** PhishGuardEdge kiểm soát lưu lượng đi qua CloudFront. File đính kèm đi thẳng từ trình duyệt đến S3 bằng form presigned POST có ràng buộc kích thước và loại tệp.
- **Mạng Lambda & Xử lý QR:** Các hàm Lambda sử dụng mạng do AWS quản lý trong template SAM này. Bước giải mã QR đọc URL ngay trong ảnh tại Lambda, lược bỏ query và fragment khi hiển thị rồi chuyển chỉ dấu sang Triage.
