---
title: "Kiểm thử OCR và QR"
date: 2026-11-24
weight: 3
chapter: false
pre: " <b> 5.5.3. </b> "
---

Đăng nhập bằng tài khoản reporter qua CloudFront và tải lần lượt từng ảnh từ thư mục lab/fixtures/qr/, sau đó chạy mẫu DEMO_1 để kiểm tra khả năng nhận diện chữ tiếng Việt và giải mã QR:

| Thứ tự gửi | Kết quả trong lần demo | Chi tiết cần đối chiếu |
| :--- | :--- | :--- |
| **1. blank.png** | Cần bổ sung (chưa chấm điểm) | Không có chữ OCR hay URL QR dùng được làm bằng chứng. |
| **2. qr-url.png** | 0/100 LOW (Không có đề xuất) | Trích xuất IOC hxxps://example[.]org/workshop/qr (đã lược query và fragment). Khi blocklist trống, một URL hợp lệ tự nó chưa cộng điểm rủi ro. |
| **3. qr-text.png** | Cần bổ sung (chưa chấm điểm) | Nội dung trong mã QR là văn bản thuần; bộ giải mã chỉ đưa URL HTTP(S) hợp lệ vào danh sách chỉ dấu. |
| **4. DEMO_1 ảnh mẫu** | 65/100 HIGH (Chờ phê duyệt) | Nhận diện chữ OCR tiếng Việt và kích hoạt đủ 3 luật (FINANCIAL_OR_OTP_REQUEST, EXEC_IMPERSONATION, URGENCY_PRESSURE). |

![Kết quả kiểm thử 4 ca ảnh OCR và QR trên Dashboard](/images/5-Workshop/5.5-Verification/5.5.3-OCR-QR/ocr-qr-rows.png)
