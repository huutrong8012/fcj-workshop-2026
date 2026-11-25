---
title: "Nhận thư qua SES"
date: 2026-11-24
weight: 1
chapter: false
pre: " <b> 5.4.1. </b> "
---

### Bước 1: Xác minh Domain Identity và cấu hình bản ghi MX

1. Mở **Amazon SES tại vùng us-east-1**, tạo Domain Identity cho tên miền (hoặc subdomain lab) của nhóm, xuất bản các bản ghi Easy DKIM lên DNS và đợi trạng thái **DKIM configuration** chuyển sang **Successful / Verified**.
2. Thêm bản ghi MX với priority 10 trỏ về inbound-smtp.us-east-1.amazonaws.com cho miền nhận thư.

![Xác minh DKIM thành công trên Amazon SES](/images/5-Workshop/5.4-Email-AI/5.4.1-SES/ses-dkim.png)

### Bước 2: Cấu hình Receipt Rule trên Amazon SES

1. Vào **Amazon SES → Email receiving → Receipt rule sets** và mở bộ rule đang active.
2. Tạo rule nhận thư cho địa chỉ SOC (ví dụ soc@<domain>) với hai action theo đúng thứ tự:
   - **Action 1 — Deliver to S3:** Chọn bucket intake của stack và đặt prefix là inbound/.
   - **Action 2 — Invoke AWS Lambda function:** Chọn hàm PhishGuard-Ingest với kiểu gọi **Event (bất đồng bộ)**.

![Cấu hình Lambda Action trong SES Receipt Rule](/images/5-Workshop/5.4-Email-AI/5.4.1-SES/ses-lambda-action.png)

### Bước 3: Kiểm thử gửi thư chuyển tiếp thực tế

Từ hộp thư Cognito đã xác minh và thuộc AllowedSenderDomains, forward một email vô hại tới soc@<domain>. Kiểm tra object mới xuất hiện trong prefix inbound/ của bucket intake và đối chiếu sự cố tương ứng trên dashboard với source_channel = SES_EMAIL và reporter_identity_status = VERIFIED_SES_DMARC.
