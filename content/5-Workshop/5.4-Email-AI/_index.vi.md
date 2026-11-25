---
title: "Tích hợp email và AI"
date: 2026-11-24
weight: 4
chapter: false
pre: " <b> 5.4. </b> "
---

Kết nối luồng nhận thư chuyển tiếp qua Amazon SES và tích hợp Amazon Bedrock Knowledge Bases (lưu chỉ mục trên S3 Vectors) cùng Bedrock Guardrails để tổng hợp kết quả phân tích có dẫn chứng playbook.

| Mục con | Kết quả cần có |
| :--- | :--- |
| **[5.4.1. Nhận thư qua SES](5.4.1-ses/)** | Lưu MIME gốc vào S3 inbound/ trước khi gọi bất đồng bộ PhishGuard-Ingest. |
| **[5.4.2. Playbook và Bedrock](5.4.2-bedrock/)** | Lập chỉ mục 5 playbook nguồn, thử truy xuất Retrieve và bật tổng hợp có Guardrails. |
