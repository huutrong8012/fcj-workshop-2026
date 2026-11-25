---
title: "Backend và xử lý ảnh"
date: 2026-11-24
weight: 1
chapter: false
pre: " <b> 5.3.1. </b> "
---

### Bước 1: Kiểm tra template.yaml và triển khai SAM Stack

Mở template.yaml để kiểm tra cấu hình vùng (us-east-1), 8 hàm Lambda, 3 bảng DynamoDB on-demand bật PITR, 5 bucket S3 thường, khóa KMS, workflow Step Functions Standard, SNS topic và SQS DLQ. Hai layer OCR tiếng Việt và giải mã QR được gắn riêng cho hàm PhishGuard-ExtractRedact.

Chạy đoạn lệnh PowerShell sau để kiểm tra cú pháp, build và deploy stack đầu tiên (nhập tên miền email của người báo cáo):

```powershell
$pgSenderDomain = Read-Host 'Mien email cua reporter, vi du congty.vn'
sam validate --lint --template-file template.yaml --region $pgRegion --profile $pgProfile
if ($LASTEXITCODE -ne 0) { throw 'Template chua hop le.' }

sam build --template-file template.yaml
if ($LASTEXITCODE -ne 0) { throw 'Build that bai.' }

sam deploy --template-file .aws-sam/build/template.yaml --stack-name $pgStack --region $pgRegion --profile $pgProfile --resolve-s3 --capabilities CAPABILITY_NAMED_IAM --parameter-overrides "AllowedSenderDomains=$pgSenderDomain" "ContainmentMode=DRY_RUN" --confirm-changeset
if ($LASTEXITCODE -ne 0) { throw 'Kiem tra su kien CloudFormation.' }
```

*(Nếu Python cục bộ thiếu thư viện native tương thích với Linux Lambda, hãy bật Docker và dùng sam build --use-container trước khi deploy).*

![CloudFormation Stack Changeset](/images/5-Workshop/5.3-Deployment/5.3.1-Backend/cfn-changeset.png)

### Bước 2: Đọc Outputs của Stack và kiểm tra Layer xử lý ảnh

Lưu các biến Outputs để sử dụng cho các bước cấu hình tiếp theo:

```powershell
function Get-PgStack {
    $raw = aws cloudformation describe-stacks --stack-name $pgStack --region $pgRegion --profile $pgProfile --output json --no-cli-pager
    if ($LASTEXITCODE -ne 0) { throw 'Khong doc duoc stack.' }
    (($raw -join "`n") | ConvertFrom-Json).Stacks[0]
}

$pg = Get-PgStack
$pgOut = @{}
foreach ($o in $pg.Outputs) { $pgOut[$o.OutputKey] = $o.OutputValue }
$pg.StackStatus
$pgOut['WebBucketName']
([uri]$pgOut['HttpApiEndpoint']).Host
```

**Mốc kiểm tra:** CloudFormation hiển thị CREATE_COMPLETE hoặc UPDATE_COMPLETE. Lưu lại WebBucketName, HttpApiEndpoint, UserPoolId, UserPoolClientId và SecOpsTopicArn.

Kiểm tra hai layer OCR và QR đã gắn vào PhishGuard-ExtractRedact (kỳ vọng bộ nhớ 512 MiB, kiến trúc x86_64 và đủ 2 layer):

```powershell
aws lambda get-function-configuration --function-name PhishGuard-ExtractRedact --region $pgRegion --profile $pgProfile --query "{Memory: MemorySize, Architecture: Architectures, Layers: Layers[*].Arn}"
```
