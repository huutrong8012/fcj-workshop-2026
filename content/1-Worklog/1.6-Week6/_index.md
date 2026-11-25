---
title: "Week 6 Worklog"
date: 2026-10-19
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---

### Week 6 Objectives:

* **Design Security Audit:** Conduct a deep-dive review of edge attack vectors, email authentication logic flaws, and approval workflow race conditions.
* **Upgrade Specification to v2.2:** Remediate 10 technical flaws identified in v2.1 (Idempotency collision, Bedrock IAM actions, SES DMARC context, NEEDS REVIEW state, domain label tokenization, and Presigned POST compensating controls).
* **Update Architecture Diagram (v2.2):** Synchronize the Draw.io architecture diagram with all v2.2 specification updates.
* **Domain & Environment Preparation:** Acquire a custom domain for Amazon SES Inbound and configure the team's AWS us-east-1 environment.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| 2 | - Upgrade idempotency enforcement: bind source intake key to the reporter's sub (TEXT#sub#idempotency-key), validate key regex, and return 409 IDEMPOTENCY KEY REUSED on content mismatch.<br>- Fix invalid bedrock:Converse IAM action to bedrock:InvokeModel + bedrock:ApplyGuardrail + bedrock:Retrieve. | 19/10/2026 | 19/10/2026 | AWS IAM Service Authorization Reference |
| 3 | - Resolve SES email authentication logic flaw: separate forwarder verification (ALLOWED SENDER DOMAINS + Cognito + dmarcVerdict == PASS) from the embedded phishing email (INDETERMINATE).<br>- Add NEEDS REVIEW state and SNS alerting for HIGH+ severity incidents lacking an automated containment target. | 20/10/2026 | 20/10/2026 | RFC 7489 (DMARC), Amazon SES Inbound Docs |
| 4 | - Refine BRAND LOOKALIKE DOMAIN algorithm: integrate Public Suffix List (.com.vn), tokenize domain labels by dot and hyphen, enforce exact token matching for short brands (acb, bidv) to avoid false positives like pacbell.net, and allow legitimate subdomains.<br>- Redact raw email addresses in claimed headers (retaining domains only). | 21/10/2026 | 21/10/2026 | Public Suffix List Specification |
| 5 | - Harden approval retry logic: handle cases where SendTaskSuccess succeeds but callback sent fails by re-reading incident state; set intake retention (3 days) longer than SQS DLQ (2 days); restrict S3 events to uploads/ prefix.<br>- Document compensating controls for direct S3 Presigned POST uploads (5 MB cap, magic-byte validation, CORS, route throttling). | 22/10/2026 | 22/10/2026 | AWS Step Functions & S3 Security Guidelines |
| 6 | - Freeze **MVP Technical Specification v2.2** and update PhishGuard Architecture v2.2.drawio (adding NotifyManualReview branch and DynamoDB/Cognito/S3 data flow arrows).<br>- Prepare custom domain and AWS us-east-1 account for infrastructure provisioning. | 23/10/2026 | 23/10/2026 | PhishGuard AI Spec v2.2 |

### Week 6 Achievements:

* Finalized **MVP Technical Specification v2.2** addressing 10 critical security and reliability issues prior to coding.
* Synchronized PhishGuard Architecture v2.2.drawio with the updated technical specification.
* Documented 11 Known Limitations of the MVP for transparent reporting in the final Workshop.
* Prepared the custom domain and us-east-1 AWS environment for active development.
