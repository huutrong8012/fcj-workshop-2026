---
title: "Portal & Access Control"
date: 2026-11-24
weight: 2
chapter: false
pre: " <b> 5.3.2. </b> "
---

### Step 1: Configure Amazon Cognito Domain

In **Amazon Cognito → User pools → PhishGuard-Users → Domain**, choose an available domain prefix in us-east-1 and record the full hosted domain (https://<prefix>.auth.us-east-1.amazoncognito.com). Ensure the public app client PhishGuard-Web uses Authorization Code Flow with PKCE.

![Cognito Hosted Domain Configuration](/images/5-Workshop/5.3-Deployment/5.3.2-Portal/cognito-domain.png)

### Step 2: Configure Amazon CloudFront Origins and Behaviors

Create or open the CloudFront distribution for the web portal using WebBucketName and the HTTP API hostname from stack outputs. Set index.html as the default root object and configure two behaviors (placing /api/* before Default (*)):

| Behavior | Origin & Protocol | Cache & Origin Request Policy |
| :--- | :--- | :--- |
| **/api/*** | <api-id>.execute-api.us-east-1.amazonaws.com (HTTPS only, empty origin path) | Allow all HTTP methods (GET, HEAD, OPTIONS, PUT, POST, PATCH, DELETE). Select **CachingDisabled** and **AllViewerExceptHostHeader** so JWT Authorization headers reach HTTP API. |
| **Default (*)** | S3 REST endpoint of WebBucketName with **Origin Access Control (OAC)**, Redirect HTTP to HTTPS | Optimized caching for static assets. |

![CloudFront /api/* Behavior Configuration](/images/5-Workshop/5.3-Deployment/5.3.2-Portal/cloudfront-api-behavior.png)

### Step 3: Update SAM Stack with CloudFront & Cognito URLs

Run the helper function below to bind OAC bucket permissions, configure S3 intake CORS for your CloudFront origin, and register Cognito callback/logout URLs:

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
    if ($LASTEXITCODE -ne 0) { throw 'Check change set and stack events.' }
}

$pgDist = (Read-Host 'CloudFront Distribution ID').Trim()
$pgSite = 'https://' + (Read-Host 'CloudFront hostname only (e.g., d1234.cloudfront.net)').Trim().TrimEnd('/')
Update-PgStack @{CloudFrontDistributionId=$pgDist; CloudFrontOrigin=$pgSite; DashboardUrl="$pgSite/"}
```

### Step 4: Create Demo Users, Confirm SNS, and Deploy React SPA

1. Create demo users and seed the 3 SHA-256 demo fixtures. Also create demo-reporter-2 in Cognito (REPORTER group) for H02 cross-user isolation tests, and confirm the SecOps email subscription in **Amazon SNS → Topics → PhishGuard-SecOps**.
   ```powershell
   .\.venv\Scripts\python.exe scripts/create_demo_users.py --profile $pgProfile
   .\.venv\Scripts\python.exe scripts/seed_demo_fixtures.py --profile $pgProfile
   ```
2. Build and sync the production React SPA to S3 and invalidate CloudFront cache:
   ```powershell
   $pgPrefix = (Read-Host 'Cognito domain prefix').Trim()
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
       if ($LASTEXITCODE -ne 0) { throw 'npm ci failed.' }
       npm run build
       if ($LASTEXITCODE -ne 0) { throw 'Frontend build failed.' }
       $web = 's3://' + $pgOut['WebBucketName'] + '/'
       aws s3 sync dist/ $web --exclude index.html --region $pgRegion --profile $pgProfile
       aws s3 cp dist/index.html ($web + 'index.html') --content-type text/html --cache-control no-cache --region $pgRegion --profile $pgProfile
   } finally { Pop-Location }

   aws cloudfront create-invalidation --distribution-id $pgDist --paths "/" "/index.html" --profile $pgProfile
   ```

**Checkpoint:** Open $pgSite, log in as a reporter, and submit a benign internal notice (*"Máy in tầng hai đang được bảo trì. Có thể dùng máy in ở tầng một."*). Verify POST /api/reports returns HTTP 202. Uploading a PNG image should return 201 from /api/uploads/presign followed by 204 from S3.
