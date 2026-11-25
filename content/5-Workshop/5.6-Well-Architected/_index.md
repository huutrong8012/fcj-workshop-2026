---
title: "Well-Architected Review"
date: 2026-11-24
weight: 6
chapter: false
pre: " <b> 5.6. </b> "
---

Select a completed demo incident and inspect three views simultaneously: the web incident detail page, the AWS Step Functions execution history, and the versioned object in the S3 evidence bucket. Together, these artifacts validate the design against the **AWS Well-Architected Framework**:

| Pillar | Design Decision in PhishGuard AI | Verification Method & Known Boundaries |
| :--- | :--- | :--- |
| **Architecture & Deployment** | CloudFront protected by WAF; web and SES paths converge at Ingest; raw evidence is stored before workflow execution; Step Functions pauses via callback for human approval. | Cross-check CloudFront behaviors, SES receipt rules, template.yaml, and Step Functions execution traces against Section 5.1. Lambdas use AWS-managed networking. |
| **Operational Excellence** | CloudFormation change sets preview stack updates; two CloudWatch alarms alert SecOps via SNS on workflow failures or non-empty DLQ. | Verify alarm states, SNS action ARNs, and CloudWatch Logs correlation using request IDs and timestamps. |
| **Security** | Cognito JWT & RBAC enforce API isolation; S3 evidence enforces SSE-KMS, Versioning, and 7-day Object Lock Governance; PII is masked before AI synthesis; Admin API mediates approvals. | Run H02 and H15 across roles; test WAF header blocking (403); enforce constrained presigned POST policies and magic-byte checks for direct S3 uploads. |
| **Reliability** | Ingest uses deterministic source_intake_key idempotency; failed async invocations route to SQS DLQ; Step Functions enforces 48-hour approval timeouts; Synthesize includes deterministic fallback. | Validate H01/H04 and H13/H14 in the demo environment. Deployed in a single AWS Region (us-east-1). |
| **Performance Efficiency** | Direct browser-to-S3 presigned POST offloads binary payloads from API Gateway; image OCR/QR uses 512 MiB x86_64 Lambda; asynchronous .waitForTaskToken eliminates idle compute during human review. | Inspect Lambda Duration, Billed Duration, and Max Memory Used in CloudWatch metrics across text vs. image payloads. |
| **Cost Optimization** | Pay-per-use serverless billing (Lambda, Step Functions, DynamoDB On-Demand, S3 Vectors); fixed monthly baseline dominated by AWS WAF (~75%). | Reconcile monthly estimates in Section 5.7 against AWS Cost Explorer exports and budget alerts. |
| **Sustainability** *(Unscored)* | Event-driven execution without idle servers; 3-day S3 lifecycle expiration on intake staging prefixes (uploads/, inbound/). | Verify S3 lifecycle rules and avoid deleting evidence objects before their 7-day governance retention expires. |
