---
title: "Week 9 Worklog"
date: 2026-11-11
weight: 9
chapter: false
pre: " <b> 1.9. </b> "
---

### Week 9 Objectives (MVP Phase 1):

* **Foundational Infrastructure Deployment (SAM Deploy):** Provision S3 buckets, DynamoDB tables, Cognito, Ingest Lambda, and the skeleton Step Functions workflow in us-east-1.
* **Early Technical Risk Validation:** Run immediate validation spikes on Vietnamese OCR accuracy, Nova Lite Vietnamese generation quality, S3 Object Lock If-None-Match, Presigned POST CORS, and live SES Inbound email forwarding.
* **End-to-End Text Ingestion Flow:** Verify text reports flow cleanly through Reporter API → intake → Ingest → evidence → Step Functions.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| 2 | - Deploy the initial AWS SAM stack to us-east-1 (S3 buckets, KMS CMK, DynamoDB tables, Cognito, SQS DLQ).<br>- Finalize SES domain verification and configure the SES Receipt Rule (S3 action to inbound/ followed by Lambda action invoking Ingest). | 11/11/2026 | 11/11/2026 | PhishGuard Spec Section 10 |
| 3 | - **OCR Decision Gate:** Benchmark Amazon Textract DetectDocumentText across 5 Vietnamese Zalo screenshots.<br>- Validate extraction accuracy on numeric strings (phone, bank account, Citizen ID) and accent-folded keywords ("chuyen khoan", "OTP", "ma xac nhan") to confirm Textract adoption. | 12/11/2026 | 12/11/2026 | PhishGuard Spec 7.2, Amazon Textract |
| 4 | - **Storage & Upload Risk Validation:** Verify PutObject conditional writes (If-None-Match: *) on the evidence bucket with Versioning + Object Lock Governance enabled.<br>- Test Presigned POST generation from Reporter API and browser file uploads against S3 CORS rules. | 13/11/2026 | 13/11/2026 | Amazon S3 Conditional Writes Docs |
| 5 | - **Vietnamese LLM Quality Validation:** Benchmark **Amazon Nova Lite** via Converse API on the 3 demo scenarios to confirm natural Vietnamese summarization, strict JSON schema adherence, and accurate 3–4 bullet recommendations. | 14/11/2026 | 14/11/2026 | PhishGuard Spec 7.6, Amazon Bedrock |
| 6 | - Implement Reporter API (POST /api/reports, POST /api/uploads/presign) and Ingest Lambda (magic-byte checks, SHA-256 hashing, source intake key idempotency, Step Functions trigger).<br>- Test forwarding a real email to soc@domain and verify dmarcVerdict == PASS maps accurately to reporter id. | 15/11/2026 | 15/11/2026 | PhishGuard Spec 2, 3, 5.3 |

### Week 9 Achievements:

* Successfully deployed the foundational **PhishGuard AI** AWS SAM infrastructure to us-east-1.
* Validated all 5 high-risk technical assumptions early:
  * Textract accurately extracted numeric identifiers and normalized Vietnamese keywords from Zalo screenshots.
  * Amazon Nova Lite produced valid JSON outputs with natural Vietnamese summaries.
  * If-None-Match: * prevented duplicate object versions on the Versioning + Object Lock Governance bucket during retries.
  * Live SES Inbound emails verified dmarcVerdict == PASS and triggered Ingest reliably.
* Established end-to-end ingestion and Step Functions execution for text-based reports.
