---
title: "Resources & Monitoring"
date: 2026-11-24
weight: 1
chapter: false
pre: " <b> 5.5.1. </b> "
---

### Automated Configuration Audit

Run tools/audit_phishguard.py from lab/ to export a read-only JSON configuration snapshot (verifying 8 Lambdas, 5 S3 buckets, 3 DynamoDB tables, 2 CloudWatch alarms, S3 Object Lock Governance 7 days, and the 512 MiB x86_64 OCR layer):

```powershell
$recipient = Read-Host 'SES lab recipient'
.\.venv\Scripts\python.exe ..	oolsudit_phishguard.py --profile $pgProfile --region us-east-1 --stack phishguard-ai --recipient $recipient --output .\phishguard-account.json
```

### Manual Console Verification Checklist

| AWS Console Service | What to Verify | Evidence to Retain |
| :--- | :--- | :--- |
| **CloudFormation (phishguard-ai)** | Account/region, bucket names, HTTP API URL, KB ID, amazon.nova-lite-v1:0, ContainmentMode=DRY_RUN, 172800s timeout. | Audit JSON snapshot and stack parameter screenshot. |
| **CloudFront (General / Security / Behaviors)** | Domain matches dashboard URL; PhishGuardEdge Web ACL (CloudFront scope) attached with managed + rate rules. | Distribution security tab screenshot showing Web ACL association. |
| **Amazon S3 (web, intake, evidence)** | Block Public Access, OAC, intake CORS for CloudFront origin, S3 event on uploads/ only, 3-day lifecycle, evidence Versioning + Governance 7d + SSE-KMS. | Bucket properties screenshots and raw_s3_version_id match. |
| **SES, SNS, Cognito, Bedrock** | Active SES receipt rule, confirmed SNS SecOps email subscription, Cognito groups/client, active KB + Guardrail version. | SES incident ID, playbook retrieval output, and Guardrail status. |
| **CloudWatch Alarms & Metrics** | PhishGuard-Workflow-Failed and PhishGuard-Ingest-DLQ-NotEmpty in OK state with SNS actions enabled. | Alarm status screenshot and UnverifiedSender metric log. |
| **Billing → Budgets** | Monthly cost budget configured with actual (80%) and forecasted (100%) email alerts. | Budget threshold and alert configuration screenshot. |

![CloudWatch Alarms Status](/images/5-Workshop/5.5-Verification/5.5.1-Monitoring/cw-alarms.png)
