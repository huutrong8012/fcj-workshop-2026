---
title: "Proposal"
date: 2026-11-24
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# PhishGuard AI — Phishing Report Intake and Approval-Gated Response on AWS

Target pilot users are employees submitting suspicious reports and the internal SecOps team. The system is deployed in **US East (N. Virginia) (us-east-1)**, with response execution operating in **DRY_RUN** mode for the demonstration build.

### 1. Real-World Problem

An employee forwards a suspicious email to a shared security mailbox, while a colleague uploads a screenshot from the same phishing campaign. During review, SecOps analysts must determine who submitted each report, which immutable original artifact was preserved, why a specific containment action was recommended, and who approved it. While a standalone chatbot can summarize individual message text, binding evidence integrity, role-based visibility, human decision gates, and audit trails together remains a heavy manual burden for operations teams.

### 2. Pilot Scope

- **Multi-Channel Intake & Evidence Preservation:** The web portal uses Amazon Cognito for authentication and accepts pasted text, screenshots, and .eml files. An Amazon SES receipt rule ingests forwarded emails from verified reporters. The Ingest function assigns a deterministic incident ID and stores the original artifact in an isolated evidence bucket.
- **PII Redaction & Controlled Raw Access:** Analysts work with PII-redacted text in daily triage. When inspecting original evidence is necessary, an administrator requests a short-lived (60-second) presigned URL, and the system records an append-only audit entry.
- **Deterministic Scoring & Playbook-Grounded AI Synthesis:** A rule-based Policy Engine calculates risk scores from extracted indicators and named rules. Amazon Bedrock retrieves SecOps playbooks via Knowledge Bases and generates grounded explanations. If AI synthesis fails or is blocked by Guardrails, the incident still retains its deterministic score and falls back cleanly with an explicit fallback_reason.
- **Human-in-the-Loop Approval:** When an actionable recommendation is generated, AWS Step Functions pauses execution with a 48-hour timeout, notifies SecOps via Amazon SNS, and resumes only upon a valid administrator decision or timeout. The demo simulates containment actions (DRY_RUN), while an internal blocklist mode records confirmed domains. Direct enforcement on email gateways or firewalls is out of pilot scope.

### 3. Deliverables and Acceptance Criteria

| Phase | Deliverable | Acceptance Criteria |
| :--- | :--- | :--- |
| **Intake & Evidence Storage** | Web Portal, SES Inbound, and dedicated evidence bucket | Submit UTF-8 text and images; verify incident ID maps to S3 object version. Resubmitting the exact same request from the same reporter returns the identical incident ID. |
| **Analysis & Synthesis** | OCR, PII redaction, deterministic rules, and grounded summary | Verify Vietnamese text extraction, triggered rules, and playbook citations. Images lacking usable text/QR route to NEEDS_MORE_INFO. Record actual synthesis status (OK or FALLBACK). |
| **Approval & Audit** | Admin decision API, Step Functions callback, and audit trail | Approving a demo sample creates an audit record. Unanswered requests expire after timeout. API rejects late decisions with HTTP 409. |
| **Operations & Handover** | RBAC isolation, CloudWatch alarms, Workshop, and cost estimate | A second reporter is denied access to another user's incident. Document alarm targets, reproduction steps, and monthly cost assumptions. |

### 4. System Architecture

![PhishGuard AI System Architecture](/images/2-Proposal/architecture.png)

### 5. Known Limitations

- **Storage Governance:** S3 Object Lock Governance protects raw evidence versions for 7 days, though principals with explicit bypass permissions can override retention.
- **OCR & PII Redaction Boundaries:** Vietnamese diacritics may occasionally be misread by OCR, and rule-based PII redaction may leave personal names or physical addresses unmasked.
- **Email Authentication Context:** Headers inside forwarded .eml files are treated as user-supplied claims (INDETERMINATE), whereas SES SPF/DKIM/DMARC verdicts apply only to the outer forwarding envelope.
- **Implemented Scope vs. Production Scaling:** CloudFront attaches the PhishGuardEdge Web ACL, while browser file uploads post directly to S3 via constrained presigned POST policies. Image processing extracts Vietnamese/English OCR text and QR URLs; the Knowledge Base indexes 5 source playbooks. Detection precision beyond test fixtures requires evaluation on an independent labeled dataset.

### 6. Cost Estimate and Scaling Readiness

For a baseline workload of **300 reports over 30 days** (including 90 images and 60 inbound SES emails), the estimated monthly AWS cost is **$9.37 USD** (including AWS WAF and one Route 53 public hosted zone). AWS Cost Explorer reporting for **01–23 November 2026** rounds to **$0.00 USD** within the exported scope. Before scaling to organization-wide production, the team recommends benchmarking triage latency, false-positive rates, false-negative rates, and manual review ratios against traditional shared-mailbox triage using an independently labeled dataset.
