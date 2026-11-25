---
title: "Worklog Tuần 7"
date: 2026-10-26
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---

### Mục tiêu tuần 7:

* **Thiết kế Giao diện Người dùng (UI/UX Wireframes):** Thiết kế giao diện React SPA cho cổng báo cáo sự cố (Reporter Portal) và bảng điều khiển điều phối (SecOps & Auditor Dashboard).
* **Chuẩn hóa Hợp đồng API (API Contracts):** Định nghĩa chi tiết cấu trúc Request/Response JSON, mã lỗi HTTP (400, 403, 409, 202) cho toàn bộ 7 endpoint REST/HTTP API.
* **Xây dựng Cấu trúc Dự án & Quy chuẩn Code:** Thiết lập Git repository, cấu trúc thư mục AWS SAM, chuẩn hóa JSON schema đầu ra cho LLM và bộ dữ liệu mẫu (fixtures).
* **Khởi tạo Tài nguyên Nền tảng (Pre-deployment):** Cấu hình DNS tên miền và yêu cầu bật quyền truy cập model trên Amazon Bedrock tại us-east-1.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Thiết kế wireframe giao diện Web Portal: màn hình đăng nhập Cognito, form gửi báo cáo (dán văn bản, tải file .eml, tải ảnh chụp màn hình) và khu vực **3 nút nạp kịch bản mẫu 1-click**. | 26/10/2026 | 26/10/2026 | Figma, React SPA Design Guidelines |
| 3 | - Thiết kế wireframe Dashboard cho SECOPS ADMIN và AUDITOR: danh sách sự cố lọc theo trạng thái (PENDING APPROVAL, NEEDS REVIEW), bảng bằng chứng đã che PII, trích dẫn RAG, nút xem bằng chứng gốc (Presigned GET 60s) và nút phê duyệt/từ chối (APPROVE/REJECT). | 27/10/2026 | 27/10/2026 | SecOps Dashboard UX Patterns |
| 4 | - Viết tài liệu đặc tả API Contract (OpenAPI/Markdown) cho 7 endpoint: POST /api/uploads/presign, POST /api/reports, GET /api/incidents, GET /api/incidents/{id}, GET /api/incidents/{id}/raw-evidence-url, POST /api/incidents/{id}/decision, GET /api/audit-logs, GET /api/blocklist. | 28/10/2026 | 28/10/2026 | OpenAPI 3.0 Specification |
| 5 | - Khởi tạo Git monorepo chia module rõ ràng: infra/ (AWS SAM template), backend/ (8 Lambda functions + shared libraries), frontend/ (React SPA), kb-docs/ (tài liệu RAG) và fixtures/ (3 mẫu demo + 5 ảnh Zalo tiếng Việt để test OCR). | 29/10/2026 | 29/10/2026 | AWS SAM Best Practices |
| 6 | - Cấu hình tên miền riêng, thiết lập bản ghi MX và gửi yêu cầu xác minh miền (Domain Identity) trên Amazon SES tại us-east-1.<br>- Kích hoạt quyền truy cập (Model Access) cho **Amazon Nova Lite** và **Titan Text Embeddings V2** trên Amazon Bedrock. | 30/10/2026 | 30/10/2026 | Amazon SES & Bedrock Console |

### Kết quả đạt được tuần 7:

* Hoàn thành bản thiết kế UI/UX cho toàn bộ luồng tương tác của 3 vai trò (REPORTER, SECOPS ADMIN, AUDITOR).
* Chốt tài liệu API Contract giúp nhóm Backend và Frontend phát triển song song mà không bị xung đột định dạng dữ liệu.
* Khởi tạo cấu trúc mã nguồn chuẩn hóa và bộ dữ liệu kiểm thử thực tế (ảnh Zalo tiếng Việt, mẫu email giả mạo, mẫu Prompt Injection).
* Hoàn tất xác minh tên miền SES và bật quyền truy cập các model Bedrock tại us-east-1.
