---
title: "Proposal"
date: 2026-11-24
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# PhishGuard AI — Tiếp nhận báo cáo phishing và phê duyệt xử lý trên AWS

Đối tượng thí điểm là nhân viên gửi báo cáo và nhóm SecOps nội bộ. Hệ thống triển khai ở **US East (N. Virginia) (us-east-1)**, chế độ xử lý trong bản demo là **DRY_RUN**.

### 1. Vấn đề thực tế

Một nhân viên chuyển tiếp email đáng ngờ vào hộp thư chung, trong khi người khác gửi ảnh chụp màn hình thuộc cùng một chiến dịch lừa đảo. Khi xem lại, chuyên viên SecOps cần biết ai đã gửi từng báo cáo, bản gốc nào đã được lưu giữ, vì sao hệ thống đưa ra đề xuất xử lý và ai là người phê duyệt. Một chatbot đơn lẻ có thể hỗ trợ đọc nội dung thư, nhưng việc gắn kết bằng chứng gốc, phân quyền xem, cổng phê duyệt của con người và nhật ký kiểm toán vẫn là gánh nặng thủ công cho đội vận hành.

### 2. Phạm vi thí điểm

- **Tiếp nhận đa kênh & Lưu giữ chứng cứ:** Portal sử dụng Amazon Cognito để xác thực, tiếp nhận văn bản, ảnh chụp màn hình và file .eml. Receipt rule của Amazon SES tiếp nhận thư chuyển tiếp từ người báo cáo đã xác minh. Hàm Ingest cấp mã sự cố tất định và lưu phiên bản gốc vào bucket bằng chứng riêng.
- **Che PII & Kiểm soát truy cập bản gốc:** Chuyên viên xem bản đã che PII trong công việc hằng ngày. Khi cần đối chiếu bản gốc, quản trị viên xin URL có thời hạn ngắn (60 giây) và hệ thống ghi nhận vào nhật ký kiểm toán chỉ thêm (AuditLogs).
- **Chấm điểm tất định & Tổng hợp có căn cứ:** Bộ luật Policy Engine chấm điểm dựa trên các chỉ dấu trích xuất và các luật nêu tên. Amazon Bedrock truy xuất playbook từ Knowledge Base và giải thích kết quả. Khi khâu tổng hợp gặp lỗi hoặc bị Guardrails chặn, hồ sơ vẫn giữ nguyên điểm theo luật và chuyển sang trạng thái FALLBACK kèm lý do rõ ràng.
- **Phê duyệt có người kiểm soát:** Khi có đề xuất hành động, AWS Step Functions đặt hạn duyệt (48 giờ), thông báo SecOps qua Amazon SNS và chỉ tiếp tục khi nhận quyết định hợp lệ từ quản trị viên hoặc hết hạn. Bản demo mô phỏng hành động (DRY_RUN), còn chế độ danh sách nội bộ lưu lại miền đã xác nhận. Tích hợp chặn cưỡng chế trên gateway hoặc firewall nằm ngoài phạm vi thí điểm.

### 3. Sản phẩm và tiêu chí nghiệm thu

| Giai đoạn | Sản phẩm | Cách nghiệm thu |
| :--- | :--- | :--- |
| **Tiếp nhận và lưu chứng cứ** | Portal, SES và kho bằng chứng riêng | Gửi văn bản UTF-8 và ảnh, đối chiếu mã sự cố với phiên bản S3. Gửi lại cùng yêu cầu của một reporter trả về cùng mã sự cố. |
| **Phân tích** | OCR, che PII, luật và tóm tắt có căn cứ | Kiểm tra chữ tiếng Việt, luật kích hoạt và dẫn chứng playbook. Ảnh thiếu nội dung đi vào nhánh NEEDS_MORE_INFO. Ghi trạng thái tổng hợp thực tế. |
| **Phê duyệt** | Quyết định admin, callback và audit | Duyệt mẫu demo tạo bản ghi audit. Yêu cầu bỏ qua sẽ hết hạn (ESCALATED_TIMEOUT). API từ chối quyết định muộn bằng HTTP 409. |
| **Vận hành và bàn giao** | Phân quyền, cảnh báo, Workshop và dự toán | Reporter thứ hai bị từ chối khi đọc sự cố người khác. Chỉ ra đích của alarm, cách dựng lại và các giả định chi phí. |

### 4. Kiến trúc hệ thống

![Kiến trúc hệ thống PhishGuard AI](/images/2-Proposal/architecture.png)

### 5. Giới hạn hệ thống

- **Bảo vệ lưu trữ:** S3 Object Lock Governance bảo vệ phiên bản raw trong 7 ngày, ngoại trừ các principal được cấp quyền bypass.
- **Giới hạn OCR & Che PII:** OCR có thể đọc nhầm dấu tiếng Việt; bản che PII dựa trên quy tắc có thể còn sót tên người hoặc địa chỉ.
- **Ngữ cảnh xác thực email:** Header bên trong thư forward được xem là dữ liệu khai báo (INDETERMINATE), còn kết quả xác thực SPF/DKIM/DMARC của SES chỉ thuộc về lớp thư chuyển tiếp bên ngoài.
- **Phạm vi đã triển khai & Mở rộng:** CloudFront gắn Web ACL PhishGuardEdge, trình duyệt tải file trực tiếp lên S3 qua presigned POST có ràng buộc. Bước đọc ảnh xử lý OCR tiếng Việt/Anh và mã QR chứa URL; kho tri thức gồm 5 playbook nguồn. Hiệu quả phát hiện ngoài bộ mẫu cần được đo lường thêm bằng tập dữ liệu có nhãn độc lập.

### 6. Chi phí và điều kiện mở rộng

Với **300 báo cáo trong 30 ngày** (gồm 90 ảnh và 60 email đến qua SES), dự toán chi phí dịch vụ AWS là **9,37 USD/tháng** (đã tính AWS WAF và một Route 53 public hosted zone). Báo cáo Cost Explorer từ **01–23/11/2026** làm tròn thành **0,00 USD** trong phạm vi bản xuất. Trước khi mở rộng diện rộng, cần dùng tập mẫu được gán nhãn độc lập để so sánh thời gian phân loại, tỷ lệ báo nhầm, tỷ lệ bỏ sót và tỷ lệ cần người xem xét với quy trình xử lý qua hộp thư truyền thống.
