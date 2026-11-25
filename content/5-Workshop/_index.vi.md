---
title: "Workshop"
date: 2026-11-24
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# Workshop PhishGuard AI — Tiếp nhận báo cáo phishing và phê duyệt xử lý trên AWS

Một nhân viên chuyển tiếp thư nghi lừa đảo, còn đồng nghiệp gửi ảnh chụp màn hình cùng chiến dịch. SecOps cần giữ bản gốc của từng báo cáo, kiểm soát ai được quyền xem và truy lại lý do xử lý. Workshop này hướng dẫn dựng và kiểm chứng toàn bộ quy trình serverless tại **US East (N. Virginia) (us-east-1)**.

### Đầu ra mong đợi

Gửi văn bản, ảnh hoặc email; theo dấu một mã sự cố tới phiên bản bằng chứng trên S3 và kiểm tra kết quả phê duyệt mô phỏng (DRY_RUN) hoặc nhánh kết thúc tương ứng. Khi dựng mới, thực hiện theo thứ tự dưới đây. Nếu đã có bản demo, bắt đầu ở **Mục 5.5.1** để đối chiếu cấu hình rồi quay lại bước có khác biệt.

| Mục | Kết quả cần có |
| :--- | :--- |
| **[5.1. Bài toán và kiến trúc](5.1-overview/)** | Theo dõi đường đi của một báo cáo và xác định nơi lưu giữ bằng chứng gốc. |
| **[5.2. Chuẩn bị môi trường](5.2-prerequisite/)** | Chuẩn bị tài khoản AWS lab, quyền IAM, công cụ CLI và gói mã nguồn thực hành. |
| **[5.3. Triển khai ứng dụng](5.3-deployment/)** | Dựng backend SAM, portal Cognito/CloudFront và lớp bảo vệ AWS WAF ở biên. |
| **[5.4. Tích hợp email và AI](5.4-email-ai/)** | Nối đường nhận thư Amazon SES, đồng bộ 5 playbook vào Bedrock KB và thử truy xuất RAG. |
| **[5.5. Kiểm chứng triển khai](5.5-verification/)** | Kiểm tra cấu hình tài nguyên, nghiệm thu H01–H15, đọc ảnh OCR/QR và theo dấu sự cố demo. |
| **[5.6. Đánh giá Well-Architected](5.6-well-architected/)** | Đối chiếu quyết định vận hành với sáu trụ cột AWS Well-Architected. |
| **[5.7. Dự toán và chi phí thực tế](5.7-cost/)** | Tự lập dự toán tháng trên Pricing Calculator và đọc báo cáo Cost Explorer đúng kỳ. |
| **[5.8. Thu dọn tài nguyên](5.8-cleanup/)** | Dọn dẹp tài nguyên tính phí an toàn theo đúng thứ tự phụ thuộc. |
