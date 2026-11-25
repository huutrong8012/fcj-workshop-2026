---
title: "Problem & Architecture"
date: 2026-11-24
weight: 1
chapter: false
pre: " <b> 5.1. </b> "
---

### Problem Context

An employee forwards a suspicious email to the SOC mailbox, while a colleague submits a screenshot from the same phishing campaign. SecOps needs to preserve the original artifact for every report, enforce strict access boundaries over raw evidence, and audit the exact reasoning behind every containment decision. This workshop builds the pipeline in **US East (N. Virginia) (us-east-1)**.

![PhishGuard AI Architecture](/images/5-Workshop/5.1-Overview/architecture.png)

### How to Read the Architecture Diagram

1. **Edge & Portal Access:** The browser loads the React SPA through **Amazon CloudFront** (protected by **AWS WAF**), authenticates via **Amazon Cognito**, and sends JWT tokens to **API Gateway HTTP API**. Reporter API and Admin API read and write **Amazon DynamoDB** to query incident records, enforce RBAC, and persist approval decisions.
2. **Multi-Channel Report Intake:** For images or .eml files, the browser obtains a signed form and executes a direct POST to **Amazon S3 (intake)**. For pasted text and 1-click demo fixtures, Reporter API writes or copies the object into uploads/{uuid}/raw, triggering Ingest via S3 Event Notifications. Forwarded emails follow a dedicated path: **Amazon SES** stores the raw MIME message in inbound/ and asynchronously invokes Ingest with receipt authentication verdicts.
3. **Evidence Preservation & Workflow Start:** Ingest validates payload format and magic bytes, computes the SHA-256 hash, and writes the raw artifact into the **S3 evidence bucket** (configured with Versioning, SSE-KMS, and 7-day Object Lock Governance). Once stored, Ingest starts an **AWS Step Functions** execution named after the incident_id, passing raw_s3_key, raw_s3_version_id, and evidence_sha256. PII-redacted outputs are isolated in the sanitized bucket, while failed asynchronous invocations route to an **Amazon SQS DLQ** monitored by **CloudWatch Alarms**.
4. **Extraction, Deterministic Triage & AI Synthesis:** Extract & Redact reads the raw evidence, runs Vietnamese/English Tesseract OCR and ZXing-C++ QR decoding on images, and masks PII. Images lacking both usable OCR text and valid QR URLs route to NEEDS_MORE_INFO. Triage computes deterministic rule scores and checks internal blocklists. Synthesize retrieves relevant playbooks from **Amazon Bedrock Knowledge Bases** (backed by **S3 Vectors**) and invokes **Amazon Nova Lite** with **Bedrock Guardrails**; if synthesis fails or is blocked, the incident displays FALLBACK alongside deterministic rule results.
5. **Routing & Approval Gate:** When no valid containment action applies, HIGH/CRITICAL incidents route to NEEDS_REVIEW with an **Amazon SNS** alert, while lower scores terminate as NO_ACTION. When an action is proposed, PrepareApproval persists the task token and PENDING_APPROVAL status in DynamoDB before publishing to SNS, pausing the Step Functions execution for up to 48 hours.
6. **Human Decision & Simulated Containment:** An administrator reviews the evidence on the web dashboard and submits APPROVE or REJECT. Admin API validates permissions and state before calling SendTaskSuccess. Rejections transition to REJECTED, timeouts to ESCALATED_TIMEOUT, and approvals in DRY_RUN mode record simulated execution outcomes and append-only audit logs.

### Key Architectural Boundaries to Highlight

- **WAF Boundary vs. Direct Upload:** PhishGuardEdge WAF inspects traffic passing through CloudFront (/, /api/*). Browser file uploads post directly to S3 via constrained presigned POST policies.
- **Networking & QR Handling:** Lambda functions run in AWS-managed networking within this SAM template. QR decoding extracts URLs locally inside Lambda, strips query strings and fragments for display, and passes canonical indicators to Triage.
