---
title: "Cost Estimate & Actuals"
date: 2026-11-24
weight: 7
chapter: false
pre: " <b> 5.7. </b> "
---

For **300 phishing reports over 30 days**, the estimated AWS service cost is **$9.37 USD/month** (before tax and annual domain renewal). Actual AWS Cost Explorer data for **01–23 November 2026** reports **$0.00 USD** when rounded to the nearest cent.

### 1. Workload Assumptions

- **Region & Edge:** Deployed in us-east-1 with CloudFront Pay-as-you-go and PhishGuardEdge Web ACL (1 AWS Managed Rule Group + 1 Rate-Based Rule; test probe disabled).
- **Monthly Volume:** 300 total reports (including 90 images requiring OCR/QR, 60 inbound SES emails, and 90 incidents reaching the approval gate), generating 12,000 HTTP API requests and 20,000 CloudFront HTTPS requests (1 GB transfer).
- **AI & Knowledge Base:** 300 Bedrock Nova Lite synthesis calls (2,000 input tokens and 1,000 output tokens each) plus 300 Knowledge Base vector queries.

### 2. Monthly Cost Breakdown

| Service | Primary Inputs | Estimated USD/Month |
| :--- | :--- | ---: |
| **AWS WAF** | 1 Web ACL ($5.00) + 1 managed rule group ($1.00) + 1 rate rule ($1.00) + 0.02M requests ($0.012) | **$7.0120** |
| **Amazon CloudFront** | PAYG, 1 GB data transfer & 20,000 requests within account tier | **$0.0000** |
| **AWS KMS** | 1 symmetric Customer Managed Key + 2,400 cryptographic requests | **$1.0072** |
| **Amazon Route 53** | 1 allocated public hosted zone + 1,000 standard DNS queries | **$0.5004** |
| **Amazon CloudWatch** | 2 standard alarms, 0.1 GB log ingestion, 0.05 GB-month storage | **$0.2515** |
| **AWS Lambda & HTTP API** | 13,380 x86 invocations (1,860 GB-seconds) + 12,000 HTTP API requests | **$0.0457** |
| **AWS Step Functions** | 300 Standard executions × 12 state transitions × $0.000025 | **$0.0900** |
| **Amazon Bedrock (Nova Lite)** | 0.6M input tokens × $0.06 + 0.3M output tokens × $0.24 | **$0.1080** |
| **Guardrails & Titan Embeddings** | 1,800 billable content-filter text units + 130,000 embedding tokens | **$0.2726** |
| **Amazon S3 & S3 Vectors** | 1 GB-month S3 Standard + 0.01 GB vector storage/writes + 300 vector queries | **$0.0363** |
| **Amazon DynamoDB** | On-Demand: 6,000 writes, 30,000 reads, 0.05 GB storage + PITR | **$0.0300** |
| **Amazon SES, SNS & SQS** | 60 inbound emails, 90 SNS publishes/emails, 1,000 SQS requests | **$0.0136** |
| **Cognito & AWS Budgets** | 10 monthly active users within free tier + 1 notification cost budget | **$0.0000** |
| **Total (before rounding)** | *Excludes domain registration and tax* | **$9.37** |

*(Note: Under AWS WAF's resource-based pricing effective 01/02/2027 at $0.01/hour per protected distribution, the WAF line item becomes $7.212 USD and the monthly total becomes $9.57 USD).*

### 3. Lambda Sizing Assumptions for AWS Pricing Calculator

| Lambda Group | Invocations/Month | Memory (MiB) | Avg. Duration |
| :--- | ---: | ---: | ---: |
| **Reporter API + Admin API** | 12,000 | 256 | 200 ms |
| **Ingest + Triage** | 600 | 256 | 1,000 ms |
| **Extract & Redact (Images)** | 90 | 512 | 3,000 ms |
| **Extract & Redact (Text/EML)** | 210 | 512 | 500 ms |
| **Synthesize** | 300 | 512 | 6,000 ms |
| **PrepareApproval + Containment** | 180 | 256 | 500 ms |

### 4. Lab Cost Explorer Reconciliation (01–23 November 2026)

| Reconciled Line Item | CSV Export Value |
| :--- | :--- |
| **Total Cost (01–23 Nov 2026)** | -0.0000003837 USD (rounds to **$0.00 USD**) |
| **Total Cost on 23 Nov 2026 (WAF test day)** | -0.0000000976 USD |
| **AWS WAF Column on 23 Nov 2026** | -0.0000000730 USD |
| **Bedrock / Knowledge Base Total** | 0 USD |

### 5. Cost Optimization Practices

- **Control WAF & Dashboard Overhead:** AWS WAF accounts for ~75% of the baseline estimate; keep probe rules enabled only during active testing and rely on existing CloudWatch alarms rather than paid custom dashboards during pilot runs.
- **Measure AI & OCR Per Step:** Track Extract & Redact billed duration separately for images vs. text, and monitor Bedrock token counts (InputTokenCount, OutputTokenCount) alongside Guardrail text units.
- **Lifecycle Hygiene:** Enforce automatic 3-day expiration on S3 intake staging prefixes while preserving evidence objects strictly for their 7-day governance retention period.
