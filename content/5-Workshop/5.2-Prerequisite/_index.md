---
title: "Environment Preparation"
date: 2026-11-24
weight: 2
chapter: false
pre: " <b> 5.2. </b> "
---

### Account, Region, and Permissions

- Use an AWS lab account in **US East (N. Virginia) (us-east-1)**. Physical names for certain Lambda functions, DynamoDB tables, Cognito user pool groups, and IAM roles are fixed in the SAM template. Use a dedicated lab account when deploying from scratch.
- **Required Tools:** Install **Git**, **Python 3.12 (x64)**, **Node.js 22 LTS**, **AWS CLI v2**, and **AWS SAM CLI**. Docker is required only if building Lambda dependencies inside a Linux container (sam build --use-container).
- **IAM & Billing Permissions:** The deploying identity needs permissions to manage CloudFormation stacks, package artifacts to S3, create IAM roles/policies, and provision serverless/AI services. The billing owner needs access to **AWS Bills**, **Cost Explorer**, and **AWS Budgets**.
- **Domain & Test Mailboxes:** Prepare DNS management access for a domain or subdomain to receive inbound emails via Amazon SES. Prepare three team-owned email addresses for reporter, admin, and auditor, plus a second reporter address (demo-reporter-2) for cross-user isolation testing.

### Local Lab Setup & Verification

Configure an AWS CLI profile named phishguard for deployment, then extract PhishGuard-Lab.zip (lab/ contains application and infrastructure code; tools/ contains audit scripts). Run the following PowerShell commands inside lab/:

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

**Checkpoint:** Confirm the AWS Account ID matches your lab account, Python is 3.12, and Test-Path returns True. (If using IAM Identity Center and the session expired, run aws sso login --profile phishguard).

Next, verify the QR decoding layer and the 5 source playbooks in the lab package (expected outputs: True and 5):

```powershell
Test-Path .\layers\qr-py312-x86_64.zip
(Get-ChildItem .\kb-docs\playbooks\*.txt).Count
```
