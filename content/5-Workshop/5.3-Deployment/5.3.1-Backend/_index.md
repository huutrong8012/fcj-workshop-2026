---
title: "Backend & Image Processing"
date: 2026-11-24
weight: 1
chapter: false
pre: " <b> 5.3.1. </b> "
---

### Step 1: Inspect template.yaml and Deploy the SAM Stack

Review template.yaml to verify the region (us-east-1), 8 Lambda functions, 3 on-demand DynamoDB tables with PITR, 5 S3 buckets, KMS CMK, Standard Step Functions workflow, SNS topic, and SQS DLQ. Note that layers/ocr-vie-py312-x86_64.zip and the QR layer attach specifically to PhishGuard-ExtractRedact.

Run the following PowerShell block to validate, build, and deploy the initial stack (entering your allowed reporter email domain):

```powershell
$pgSenderDomain = Read-Host 'Reporter email domain (e.g., example.com)'
sam validate --lint --template-file template.yaml --region $pgRegion --profile $pgProfile
if ($LASTEXITCODE -ne 0) { throw 'Invalid SAM template.' }

sam build --template-file template.yaml
if ($LASTEXITCODE -ne 0) { throw 'SAM build failed.' }

sam deploy --template-file .aws-sam/build/template.yaml --stack-name $pgStack --region $pgRegion --profile $pgProfile --resolve-s3 --capabilities CAPABILITY_NAMED_IAM --parameter-overrides "AllowedSenderDomains=$pgSenderDomain" "ContainmentMode=DRY_RUN" --confirm-changeset
if ($LASTEXITCODE -ne 0) { throw 'Check CloudFormation stack events.' }
```

*(If local Python lacks Linux-compatible native dependencies, start Docker and run sam build --use-container before deploying).*

![CloudFormation Stack Changeset](/images/5-Workshop/5.3-Deployment/5.3.1-Backend/cfn-changeset.png)

### Step 2: Capture Stack Outputs & Verify Image Processing Layers

Store the stack outputs in your session for subsequent deployment steps:

```powershell
function Get-PgStack {
    $raw = aws cloudformation describe-stacks --stack-name $pgStack --region $pgRegion --profile $pgProfile --output json --no-cli-pager
    if ($LASTEXITCODE -ne 0) { throw 'Unable to describe stack.' }
    (($raw -join "`n") | ConvertFrom-Json).Stacks[0]
}

$pg = Get-PgStack
$pgOut = @{}
foreach ($o in $pg.Outputs) { $pgOut[$o.OutputKey] = $o.OutputValue }
$pg.StackStatus
$pgOut['WebBucketName']
([uri]$pgOut['HttpApiEndpoint']).Host
```

**Checkpoint:** Confirm StackStatus is CREATE_COMPLETE or UPDATE_COMPLETE, and record WebBucketName, HttpApiEndpoint, UserPoolId, UserPoolClientId, and SecOpsTopicArn.

Verify the OCR and QR layers attached to PhishGuard-ExtractRedact (expected: 2 layer ARNs, 512 MiB memory, x86_64 architecture):

```powershell
aws lambda get-function-configuration --function-name PhishGuard-ExtractRedact --region $pgRegion --profile $pgProfile --query "{Memory: MemorySize, Architecture: Architectures, Layers: Layers[*].Arn}"
```
