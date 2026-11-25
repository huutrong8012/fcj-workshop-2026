---
title: "Portal và phân quyền"
date: 2026-11-24
weight: 2
chapter: false
pre: " <b> 5.3.2. </b> "
---

### Bước 1: Cấu hình Domain cho Amazon Cognito

Trong **Amazon Cognito → User pools → PhishGuard-Users → Domain**, chọn một prefix còn trống tại vùng us-east-1 và lưu lại địa chỉ đầy đủ (https://<prefix>.auth.us-east-1.amazoncognito.com). Đảm bảo app client PhishGuard-Web sử dụng Authorization Code Flow kèm PKCE.

![Cấu hình Cognito Domain](/images/5-Workshop/5.3-Deployment/5.3.2-Portal/cognito-domain.png)

### Bước 2: Cấu hình Origins và Behaviors trên CloudFront

Tạo hoặc mở CloudFront distribution cho giao diện web bằng WebBucketName và hostname HTTP API từ Outputs. Đặt default root object là index.html và cấu hình hai behavior (đặt /api/* phía trên Default (*)):

| Behavior | Origin và giao thức | Cache và Origin Request Policy |
| :--- | :--- | :--- |
| **/api/*** | <api-id>.execute-api.us-east-1.amazonaws.com (HTTPS only, origin path để trống) | Cho phép mọi HTTP method (GET, HEAD, OPTIONS, PUT, POST, PATCH, DELETE). Chọn **CachingDisabled** và **AllViewerExceptHostHeader** để header Authorization (JWT) truyền tới HTTP API. |
| **Default (*)** | Endpoint S3 REST của WebBucketName dùng **Origin Access Control (OAC)**, Redirect HTTP sang HTTPS | Cache tối ưu cho tài nguyên tĩnh. |

![Cấu hình CloudFront Behavior /api/*](/images/5-Workshop/5.3-Deployment/5.3.2-Portal/cloudfront-api-behavior.png)

### Bước 3: Cập nhật SAM Stack với URL của CloudFront & Cognito

Chạy hàm PowerShell dưới đây để cấp quyền OAC đọc web bucket, mở CORS trên intake cho origin CloudFront và đăng ký URL callback/logout trên Cognito:

```powershell
function Update-PgStack([hashtable] $changes) {
    $all = @{}
    foreach ($p in (Get-PgStack).Parameters) {
        if ($null -ne $p.ParameterValue -and "$($p.ParameterValue)" -ne '') {
            $all[$p.ParameterKey] = [string]$p.ParameterValue
        }
    }
    foreach ($name in $changes.Keys) { $all[$name] = [string]$changes[$name] }
    $overrides = @(foreach ($name in $all.Keys) { "$name=$($all[$name])" })
    sam deploy --template-file .aws-sam/build/template.yaml --stack-name $pgStack --region $pgRegion --profile $pgProfile --resolve-s3 --capabilities CAPABILITY_NAMED_IAM --parameter-overrides @overrides --confirm-changeset --no-fail-on-empty-changeset
    if ($LASTEXITCODE -ne 0) { throw 'Kiem tra change set va su kien stack.' }
}

$pgDist = (Read-Host 'CloudFront distribution ID').Trim()
$pgSite = 'https://' + (Read-Host 'Chi nhap hostname CloudFront').Trim().TrimEnd('/')
Update-PgStack @{CloudFrontDistributionId=$pgDist; CloudFrontOrigin=$pgSite; DashboardUrl="$pgSite/"}
```

### Bước 4: Tạo người dùng Demo, xác nhận SNS và Deploy React SPA

1. Tạo tài khoản demo và nạp 3 mẫu kiểm thử cố định theo SHA-256. Tạo thêm user demo-reporter-2 trong Cognito (nhóm REPORTER) phục vụ bài kiểm thử H02, đồng thời mở email xác nhận subscription trong **Amazon SNS → Topics → PhishGuard-SecOps**.
   ```powershell
   .\.venv\Scripts\python.exe scripts/create_demo_users.py --profile $pgProfile
   .\.venv\Scripts\python.exe scripts/seed_demo_fixtures.py --profile $pgProfile
   ```
2. Build và đồng bộ giao diện React SPA lên S3, sau đó tạo invalidation trên CloudFront:
   ```powershell
   $pgPrefix = (Read-Host 'Prefix Cognito da tao').Trim()
   $pgEvidence = $pgOut['EvidenceBucketName']
   $envFile = @"
   VITE_USE_MOCK=false
   VITE_COGNITO_REGION=$pgRegion
   VITE_COGNITO_USER_POOL_ID=$($pgOut['UserPoolId'])
   VITE_COGNITO_CLIENT_ID=$($pgOut['UserPoolClientId'])
   VITE_COGNITO_DOMAIN=https://$pgPrefix.auth.$pgRegion.amazoncognito.com
   VITE_COGNITO_REDIRECT_URI=$pgSite/
   VITE_COGNITO_LOGOUT_URI=$pgSite/
   VITE_COGNITO_SCOPES=openid email profile
   VITE_EVIDENCE_ALLOWED_HOSTS=$pgEvidence.s3.$pgRegion.amazonaws.com,$pgEvidence.s3.amazonaws.com
   "@
   [IO.File]::WriteAllText((Join-Path $PWD 'frontend/.env.production.local'), $envFile)

   Push-Location frontend
   try {
       npm ci
       if ($LASTEXITCODE -ne 0) { throw 'npm ci that bai.' }
       npm run build
       if ($LASTEXITCODE -ne 0) { throw 'Build web that bai.' }
       $web = 's3://' + $pgOut['WebBucketName'] + '/'
       aws s3 sync dist/ $web --exclude index.html --region $pgRegion --profile $pgProfile
       aws s3 cp dist/index.html ($web + 'index.html') --content-type text/html --cache-control no-cache --region $pgRegion --profile $pgProfile
   } finally { Pop-Location }

   aws cloudfront create-invalidation --distribution-id $pgDist --paths "/" "/index.html" --profile $pgProfile
   ```

**Mốc kiểm tra:** Mở URL CloudFront, đăng nhập và gửi thông báo nội bộ *"Máy in tầng hai đang được bảo trì. Có thể dùng máy in ở tầng một."* Trong tab Network, POST /api/reports trả về mã 202. Với ảnh PNG tải lên, /api/uploads/presign trả về 201 và S3 trả về 204.
