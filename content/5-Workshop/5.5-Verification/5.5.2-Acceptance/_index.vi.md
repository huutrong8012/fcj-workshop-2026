---
title: "Nghiệm thu H01–H15"
date: 2026-11-24
weight: 2
chapter: false
pre: " <b> 5.5.2. </b> "
---

### Chạy bộ Script kiểm thử tự động

Chạy các script kiểm thử cục bộ và trên môi trường AWS thực tế từ thư mục lab/:

```powershell
.\.venv\Scripts\python.exe scripts/check_phase_h_local.py
.\.venv\Scripts\python.exe scripts/check_phase_h_live.py --profile $pgProfile --other-reporter demo-reporter-2
.\.venv\Scripts\python.exe scripts/verify_group1_upload.py --profile $pgProfile
```

![Kết quả Terminal kiểm thử H01–H03](/images/5-Workshop/5.5-Verification/5.5.2-Acceptance/h01-h03-terminal.png)

### Bảng tiêu chí nghiệm thu 15 ca (H01–H15)

| Ca | Kết quả cần đối chiếu |
| :--- | :--- |
| **H01** | Cùng reporter, key và nội dung trả về cùng incident_id (INC-8066dcaaa5e4) và một execution mang tên theo ID. |
| **H02** | Hai reporter dùng cùng key/nội dung tạo ra hai incident độc lập; đọc chéo bị từ chối cả hai chiều (403/404). |
| **H03** | Dùng lại key cho nội dung khác trả về HTTP 409; key sai định dạng trả về HTTP 400. |
| **H04** | Giả lập gián đoạn sau khi ghi evidence; thử lại bảo toàn một phiên bản bằng chứng và một lần khởi tạo workflow. |
| **H05** | File .eml có Return-Path khác From ghi nhận xác thực gốc INDETERMINATE, không suy diễn AUTH_DMARC_FAIL. |
| **H06** | Thư SES từ người gửi chưa xác minh ghi log UNVERIFIED_SENDER, tăng metric UnverifiedSender và không tạo incident. |
| **H07** | Câu cảnh báo ngân hàng nhắc giữ kín OTP chấm 0 điểm trên mẫu chuẩn (NO_ACTION). |
| **H08** | Câu cảnh báo đi kèm liên kết yêu cầu nhập OTP kích hoạt luật FINANCIAL_OR_OTP_REQUEST (+30). |
| **H09** | Ảnh trắng chuyển sang NEEDS_MORE_INFO và để trống điểm rủi ro. |
| **H10** | So sánh DEMO_2 và DEMO_3: mẫu 3 gắn cờ PROMPT_INJECTION_DETECTED nhưng giữ nguyên điểm 80/100. |
| **H11** | Báo cáo mức HIGH nhưng thiếu tên miền mục tiêu để chặn chuyển sang NEEDS_REVIEW và gửi cảnh báo SNS. |
| **H12** | Năm hostname trong bộ kiểm thử cục bộ cho kết quả nhận diện tên miền giả mạo chính xác. |
| **H13** | Quyết định lặp cùng decision_id được xử lý nhất quán, chỉ tạo một bản ghi audit và một callback thành công. |
| **H14** | Hết thời gian phê duyệt chuyển trạng thái ESCALATED_TIMEOUT; quyết định gửi muộn bị từ chối bằng HTTP 409. |
| **H15** | REPORTER và AUDITOR chỉ nhận dữ liệu theo quyền; phản hồi API và SNS không lộ sfn_task_token, raw_s3_key hay header gốc. |

### Minh chứng chi tiết các ca trọng tâm (H05, H11, H14)

1. **Ca H05 (Xác thực email khai báo INDETERMINATE) & Nhánh Step Functions NO_ACTION:**
   ![Kết quả xác thực email H05](/images/5-Workshop/5.5-Verification/5.5.2-Acceptance/h05-indeterminate.png)
   ![Luồng Step Functions MarkNoAction](/images/5-Workshop/5.5-Verification/5.5.2-Acceptance/sfn-no-action.png)
2. **Ca H11 (Chuyển kiểm tra thủ công NEEDS_REVIEW):** Tải file fixtures/phase-h/H11-manual-review.eml qua Presigned POST; kiểm tra dashboard hiển thị 50/100 HIGH (Cần kiểm tra) kèm 3 luật kích hoạt và trích dẫn playbook RAG:
   ![Tải file H11-manual-review.eml](/images/5-Workshop/5.5-Verification/5.5.2-Acceptance/h11-upload.png)
   ![Dòng sự cố H11 trên Dashboard](/images/5-Workshop/5.5-Verification/5.5.2-Acceptance/h11-row.png)
   ![Luật kích hoạt và nguồn tham chiếu RAG H11](/images/5-Workshop/5.5-Verification/5.5.2-Acceptance/h11-detail.png)
3. **Ca H14 (Quá hạn duyệt & Từ chối quyết định muộn 409):** Đổi tạm ApprovalTimeoutSeconds=60, chờ mẫu DEMO_1 quá hạn (ESCALATED_TIMEOUT), chạy check_phase_h_live.py --expired <incident-id> xác nhận H14_LATE_DECISION_409: PASS rồi khôi phục timeout về 172800:
   ![Dòng sự cố quá hạn duyệt H14](/images/5-Workshop/5.5-Verification/5.5.2-Acceptance/h14-timeout-row.png)
   ![Kết quả Terminal H14_LATE_DECISION_409](/images/5-Workshop/5.5-Verification/5.5.2-Acceptance/h14-409-terminal.png)
