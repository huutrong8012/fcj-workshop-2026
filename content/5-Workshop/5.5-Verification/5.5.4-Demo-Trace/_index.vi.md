---
title: "Theo dấu một sự cố demo"
date: 2026-11-24
weight: 4
chapter: false
pre: " <b> 5.5.4. </b> "
---

### Bộ kịch bản mẫu chuẩn (intake/fixtures/)

Các mẫu nạp sẵn giúp người kiểm thử chạy trên cùng bản deploy có thể đối chiếu trên cùng một dữ liệu đầu vào:

![Ba kịch bản demo 1-click trên Portal](/images/5-Workshop/5.5-Verification/5.5.4-Demo-Trace/demo-fixtures.png)

| Mẫu kịch bản | Đường đi cần trình diễn | Dữ kiện và kết quả chốt |
| :--- | :--- | :--- |
| **1. Ảnh giả danh lãnh đạo (DEMO_1)** | Đăng nhập reporter, chạy mẫu ảnh, mở nội dung OCR và từng luật. Chuyển sang SECOPS_ADMIN, đọc mục tiêu/chế độ rồi bấm duyệt. | Kỳ vọng 65/100 HIGH, đề xuất FINANCE_VERIFICATION_ALERT, gửi SNS, ghi audit và kết thúc ở trạng thái SIMULATED. |
| **2. Email giả mạo ngân hàng (DEMO_2)** | Mở IOC đã defang (login.techc0mbank-vn[.]com), giải thích luật giả mạo thương hiệu và duyệt đề xuất BLOCK_DOMAIN. | Kỳ vọng 80/100 CRITICAL, đề xuất BLOCK_DOMAIN (DRY_RUN), ghi nhận APPROVED_DRY_RUN vào danh sách chặn nội bộ. |
| **3. Email chứa Prompt Injection (DEMO_3)** | Đối chiếu luật và điểm với mẫu 2; kiểm tra cờ PROMPT_INJECTION_DETECTED, trạng thái tổng hợp và quyền hạn của Synthesize. | Điểm rủi ro giữ nguyên 80/100 CRITICAL; model không thể tự sửa điểm hay gọi hàm Containment. |

### Kiểm chứng đối chứng OCR tiếng Việt

Gửi thông báo bảo trì máy in nội bộ chứng minh hệ thống nhận diện chính xác dấu tiếng Việt và trả về mức rủi ro thấp 0/100 LOW (Không có đề xuất):

![Ảnh đối chứng OCR thông báo bảo trì máy in](/images/5-Workshop/5.5-Verification/5.5.4-Demo-Trace/printer-notice.png)
