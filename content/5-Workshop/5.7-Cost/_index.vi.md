---
title: "Dự toán và chi phí thực tế"
date: 2026-11-24
weight: 7
chapter: false
pre: " <b> 5.7. </b> "
---

Với **300 báo cáo trong 30 ngày**, chi phí dịch vụ dự kiến của PhishGuard AI là **9,37 USD/tháng** (chưa tính thuế và phí gia hạn tên miền). Dữ liệu thực tế trên AWS Cost Explorer giai đoạn **01–23/11/2026** ghi nhận **0,00 USD** khi làm tròn đến cent.

### 1. Phạm vi và giả định của dự toán

- **Region & Lớp biên:** Triển khai tại us-east-1, dùng CloudFront Pay-as-you-go và Web ACL PhishGuardEdge (1 AWS managed rule group tiêu chuẩn + 1 rate-based rule; tắt probe sau khi kiểm thử).
- **Sản lượng giả định:** 300 báo cáo/tháng (gồm 90 ảnh cần OCR/QR, 60 email đến qua SES và 90 ca đi đến bước quyết định), phát sinh 12.000 request HTTP API và 20.000 request HTTPS qua CloudFront (1 GB dữ liệu).
- **AI & Knowledge Base:** 300 lượt tổng hợp bằng Bedrock Nova Lite (2.000 token đầu vào, 1.000 token đầu ra/lượt) và 300 truy vấn Knowledge Base trên S3 Vectors.

### 2. Bảng dự toán chi phí một tháng

| Khoản dịch vụ | Đầu vào chính | USD/tháng |
| :--- | :--- | ---: |
| **AWS WAF** | 1 Web ACL ($5,00) + 1 managed group ($1,00) + 1 rate rule ($1,00) + 0,02 triệu request ($0,012) | **7,0120** |
| **Amazon CloudFront** | PAYG, 1 GB băng thông và 20.000 request trong hạn mức tài khoản | **0,0000** |
| **AWS KMS** | 1 khóa đối xứng Customer Managed Key + 2.400 lượt gọi mã hóa | **1,0072** |
| **Amazon Route 53** | 1 public hosted zone + 1.000 truy vấn DNS chuẩn | **0,5004** |
| **Amazon CloudWatch** | 2 alarm tiêu chuẩn, nạp 0,1 GB log và lưu trữ 0,05 GB-tháng | **0,2515** |
| **AWS Lambda & HTTP API** | 13.380 lượt gọi Lambda x86 (1.860 GB-giây) + 12.000 HTTP API request | **0,0457** |
| **AWS Step Functions** | 300 Standard execution × 12 lần chuyển trạng thái × $0,000025 | **0,0900** |
| **Amazon Bedrock (Nova Lite)** | 0,6 triệu token vào × $0,06 + 0,3 triệu token ra × $0,24 | **0,1080** |
| **Guardrails & Titan Embeddings** | 1.800 text unit lọc nội dung + 130.000 token embedding | **0,2726** |
| **Amazon S3 & S3 Vectors** | 1 GB-tháng S3 Standard + 0,01 GB vector + 300 truy vấn vector nhỏ | **0,0363** |
| **Amazon DynamoDB** | On-Demand: 6.000 đơn vị ghi, 30.000 đơn vị đọc, 0,05 GB lưu trữ + PITR | **0,0300** |
| **Amazon SES, SNS & SQS** | 60 thư đến, 90 lượt publish/email SNS, 1.000 request SQS | **0,0136** |
| **Cognito & AWS Budgets** | 10 người dùng hoạt động trong hạn mức miễn phí + 1 cost budget cảnh báo | **0,0000** |
| **Tổng tính trước làm tròn** | *Chưa bao gồm phí tên miền và thuế* | **9,37** |

*(Ghi chú: Từ kỳ giá 01/02/2027 khi AWS WAF tính theo tài nguyên bảo vệ ở mức $0,01/giờ/distribution, dòng WAF là 7,212 USD và tổng dự toán tháng là 9,57 USD).*

### 3. Bảng thông số Lambda nhập vào AWS Pricing Calculator

| Nhóm hàm Lambda | Số lần/tháng | Bộ nhớ (MiB) | Thời lượng trung bình |
| :--- | ---: | ---: | ---: |
| **Reporter API + Admin API** | 12.000 | 256 | 200 ms |
| **Ingest + Triage** | 600 | 256 | 1.000 ms |
| **Extract & Redact (Xử lý ảnh)** | 90 | 512 | 3.000 ms |
| **Extract & Redact (Văn bản/EML)** | 210 | 512 | 500 ms |
| **Synthesize** | 300 | 512 | 6.000 ms |
| **PrepareApproval + Containment** | 180 | 256 | 500 ms |

### 4. Đối chiếu dữ liệu Cost Explorer kỳ chạy lab (01–23/11/2026)

| Hạng mục đối chiếu | Kết quả đọc từ file CSV |
| :--- | :--- |
| **Tổng chi phí từ 01–23/11/2026** | -0,0000003837 USD (làm tròn thành **0,00 USD**) |
| **Tổng chi phí riêng ngày 23/11/2026** | -0,0000000976 USD |
| **Cột AWS WAF ngày 23/11/2026** | -0,0000000730 USD |
| **Bedrock / Knowledge Base cả kỳ** | 0 USD |

### 5. Tối ưu chi phí mà vẫn giữ được bằng chứng

- **Kiểm soát chi phí WAF & Dashboard:** AWS WAF chiếm khoảng 75% chi phí cơ sở; chỉ bật luật probe trong lúc kiểm thử và tận dụng 2 CloudWatch alarm sẵn có thay vì tạo dashboard trả phí khi lượng truy cập còn nhỏ.
- **Đo lường AI & OCR theo từng bước:** Tách riêng thời lượng chạy Extract & Redact giữa ảnh và văn bản, đồng thời giám sát số token Bedrock và text unit của Guardrails.
- **Dọn dẹp theo vòng đời dữ liệu:** Duy trì rule tự xóa dữ liệu tạm ở bucket intake sau 3 ngày và chỉ giữ bằng chứng ở evidence đúng thời hạn bảo toàn 7 ngày.
