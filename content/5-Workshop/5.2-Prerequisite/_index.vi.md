---
title: "Chuẩn bị môi trường"
date: 2026-11-24
weight: 2
chapter: false
pre: " <b> 5.2. </b> "
---

### Tài khoản, Region và Phân quyền

- Sử dụng một tài khoản AWS lab tại **US East (N. Virginia) (us-east-1)**. Tên vật lý của một số hàm Lambda, bảng DynamoDB, nhóm Cognito và IAM role được cố định trong mẫu SAM. Nên dùng một tài khoản lab riêng khi dựng độc lập từ đầu.
- **Công cụ cần cài đặt:** **Git**, **Python 3.12 (x64)**, **Node.js 22 LTS**, **AWS CLI v2** và **AWS SAM CLI**. Docker chỉ cần khi sử dụng phương án build trong container Linux (sam build --use-container).
- **Quyền triển khai & Billing:** Danh tính triển khai cần quyền quản lý CloudFormation stack, đóng gói S3 và tạo các tài nguyên IAM/serverless/AI. Người phụ trách Billing cần quyền đọc **Bills**, **Cost Explorer** và quản lý **Budgets**.
- **Tên miền & Hộp thư kiểm thử:** Chuẩn bị quyền chỉnh sửa bản ghi DNS trên một domain hoặc subdomain nhận thư. Chuẩn bị 3 địa chỉ email thuộc quyền quản lý của nhóm cho reporter, admin, auditor và thêm 1 email reporter thứ hai (demo-reporter-2) để kiểm thử phân tách người dùng.

### Thiết lập bộ Lab cục bộ & Mốc kiểm tra

Cấu hình AWS CLI profile tên phishguard cho người triển khai, sau đó giải nén PhishGuard-Lab.zip (lab/ chứa mã thực hành; tools/ chứa công cụ kiểm tra cấu hình). Chạy các lệnh PowerShell sau trong thư mục lab/:

```powershell
Set-Location .\lab
$pgProfile = 'phishguard'
$pgRegion = 'us-east-1'
$pgStack = 'phishguard-ai'

aws sts get-caller-identity --profile $pgProfile --query '{Account: Account, Arn: Arn}'
py -3.12 -m venv .venv
.\.venv\Scripts\python.exe -m pip install -r requirements-dev.txt
.\.venv\Scripts\python.exe -m pip install "moto[s3,dynamodb]==5.2.5"
sam --version
node --version
Test-Path .\layers\ocr-vie-py312-x86_64.zip
```

**Mốc kiểm tra:** Account phải khớp tài khoản lab đã chọn, Python là phiên bản 3.12 và Test-Path trả về True. (Nếu profile dùng IAM Identity Center và phiên đã hết hạn, chạy aws sso login --profile phishguard).

Trong cùng cửa sổ terminal, kiểm tra layer giải mã QR và 5 tài liệu playbook đi kèm gói lab (kết quả mong đợi là True và 5):

```powershell
Test-Path .\layers\qr-py312-x86_64.zip
(Get-ChildItem .\kb-docs\playbooks\*.txt).Count
```
