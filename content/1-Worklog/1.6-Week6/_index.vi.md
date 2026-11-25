---
title: "Worklog Tuần 6"
date: 2026-10-19
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---

### Mục tiêu tuần 6:

* **Kiểm toán Bảo mật Thiết kế (Design Security Audit):** Rà soát sâu các kịch bản tấn công biên, lỗi logic xác thực email và lỗ hổng race condition trong quy trình phê duyệt.
* **Nâng cấp Đặc tả lên Phiên bản v2.2:** Khắc phục 10 điểm rủi ro kỹ thuật được phát hiện từ bản v2.1 (Idempotency, IAM Bedrock, SES DMARC, trạng thái NEEDS REVIEW, thuật toán tách nhãn tên miền, biện pháp bù Presigned POST).
* **Cập nhật Sơ đồ Kiến trúc (v2.2):** Đồng bộ hóa sơ đồ Draw.io với các thay đổi mới nhất trong đặc tả kỹ thuật.
* **Chuẩn bị Môi trường & Tên miền:** Đăng ký tên miền phục vụ Amazon SES Inbound và cấu hình môi trường phát triển nhóm.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Rà soát cơ chế chống trùng lặp: nâng cấp source intake key luôn gắn sub của người báo cáo (TEXT#sub#idempotency-key), kiểm tra regex định dạng và trả lỗi 409 IDEMPOTENCY KEY REUSED nếu tái dùng key với nội dung khác.<br>- Sửa quyền IAM Bedrock từ bedrock:Converse (không hợp lệ) sang bedrock:InvokeModel + bedrock:ApplyGuardrail + bedrock:Retrieve. | 19/10/2026 | 19/10/2026 | AWS IAM Service Authorization Reference |
| 3 | - Xử lý lỗ hổng logic xác thực email SES: tách bạch verdict của thư forward (chỉ dùng xác minh người báo cáo qua ALLOWED SENDER DOMAINS + Cognito + dmarcVerdict == PASS) với email lừa đảo bên trong (luôn gán INDETERMINATE).<br>- Bổ sung trạng thái NEEDS REVIEW + cảnh báo SNS cho các ca điểm HIGH trở lên nhưng không sinh được hành động thực thi hợp lệ. | 20/10/2026 | 20/10/2026 | RFC 7489 (DMARC), Amazon SES Inbound Docs |
| 4 | - Cải tiến thuật toán BRAND LOOKALIKE DOMAIN: sử dụng Public Suffix List (.com.vn), tách nhãn tên miền theo dấu chấm và gạch nối, khớp nguyên token với thương hiệu ngắn (acb, bidv) để tránh báo nhầm pacbell.net và cho qua subdomain chính thức.<br>- Che địa chỉ email thô trong claimed headers (chỉ giữ tên miền). | 21/10/2026 | 21/10/2026 | Public Suffix List Specification |
| 5 | - Gia cố quy trình duyệt: xử lý kịch bản SendTaskSuccess thành công nhưng lỗi cập nhật callback sent bằng cách đọc lại trạng thái sự cố; chỉnh thời gian lưu intake (3 ngày) dài hơn SQS DLQ (2 ngày); chỉ kích hoạt S3 event trên prefix uploads/.<br>- Bổ sung biện pháp bù cho đường upload thẳng S3 Presigned POST (giới hạn 5 MB, kiểm tra magic bytes, CORS, throttling). | 22/10/2026 | 22/10/2026 | AWS Step Functions & S3 Security Guidelines |
| 6 | - Chốt tài liệu **Đặc tả Kỹ thuật MVP v2.2** và cập nhật sơ đồ PhishGuard Architecture v2.2.drawio (thêm nhánh NotifyManualReview, bổ sung các mũi tên tương tác DynamoDB/Cognito/S3).<br>- Chuẩn bị tên miền riêng và tài khoản AWS region us-east-1 cho giai đoạn dựng hạ tầng. | 23/10/2026 | 23/10/2026 | PhishGuard AI Spec v2.2 |

### Kết quả đạt được tuần 6:

* Hoàn thiện **Đặc tả Kỹ thuật MVP v2.2** với 10 cải tiến bảo mật và độ tin cậy quan trọng, loại bỏ các kẽ hở logic trước khi bước vào lập trình.
* Đồng bộ hóa hoàn toàn sơ đồ kiến trúc PhishGuard Architecture v2.2.drawio với đặc tả kỹ thuật.
* Xác định rõ 11 giới hạn đã biết (Known Limitations) của phiên bản MVP để trình bày minh bạch trong báo cáo và Workshop.
* Sẵn sàng tên miền và môi trường AWS (us-east-1) cho giai đoạn triển khai thực tế.
