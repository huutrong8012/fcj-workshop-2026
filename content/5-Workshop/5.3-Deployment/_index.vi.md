---
title: "Triển khai ứng dụng"
date: 2026-11-24
weight: 3
chapter: false
pre: " <b> 5.3. </b> "
---

Dựng backend trước để lấy các giá trị Outputs cho portal. Khi đã có CloudFront Distribution ID, tiến hành gắn Web ACL của AWS WAF và kiểm tra lớp bảo vệ tại biên.

| Mục con | Kết quả cần có |
| :--- | :--- |
| **[5.3.1. Backend và xử lý ảnh](5.3.1-backend/)** | Dựng stack AWS SAM và kiểm tra hai layer xử lý ảnh OCR và QR. |
| **[5.3.2. Portal và phân quyền](5.3.2-portal/)** | Cấu hình Cognito domain, hai origin trên CloudFront và deploy frontend dùng API thật. |
| **[5.3.3. Bảo vệ CloudFront bằng WAF](5.3.3-waf/)** | Gắn Web ACL, chứng minh luật header chặn được (403) và kiểm thử lại portal. |
