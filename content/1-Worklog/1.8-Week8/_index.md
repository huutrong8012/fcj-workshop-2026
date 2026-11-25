---
title: "Week 8 Worklog"
date: 2026-11-02
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---

### Week 8 Objectives:

* **Core Business Logic Prototyping:** Develop and unit-test standalone pure-logic modules prior to cloud integration: PII Redactor, URL Defanger, and Deterministic Policy Engine.
* **Lookalike Domain Algorithm Verification:** Validate BRAND LOOKALIKE DOMAIN accuracy against the Public Suffix List and Vietnamese banking brand keywords.
* **AWS SAM Base Infrastructure Authoring:** Write SAM templates provisioning storage (S3, DynamoDB), identity (Cognito), and error queues (SQS DLQ).
* **Real Test Account Provisioning:** Create Cognito user accounts backed by real email addresses (avoiding .local) for end-to-end SES testing.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| 2 | - Implement the **PII Redactor** module: detect and consistently mask Citizen IDs, Vietnamese phone numbers, bank account numbers (with banking context), OTP codes, and email local-parts into relational placeholders (BANK ACCOUNT 1, EMAIL USER 1).<br>- Write unit tests verifying entity relationship preservation. | 02/11/2026 | 02/11/2026 | Python Regex & NLP Sanitization Patterns |
| 3 | - Implement the **URL Extractor & Defanger** module: safely extract URLs (strictly no active fetching), compute canonical value, and format display value (hxxp, [.]).<br>- Bundle the offline Public Suffix List and implement BRAND LOOKALIKE DOMAIN (dot/hyphen label tokenization and homoglyph normalization 0→o, 1→l, rn→m). | 03/11/2026 | 03/11/2026 | Public Suffix List, PhishGuard Spec 7.4 |
| 4 | - Implement the **Deterministic Policy Engine**: encode all 6 scoring rules, safe bank advisory exclusion logic, Prompt Injection pattern detection, and proposed actions generation (BLOCK DOMAIN, FINANCE VERIFICATION ALERT) or requires manual review flagging. | 04/11/2026 | 04/11/2026 | PhishGuard Spec 7.5 |
| 5 | - Author template.yaml in AWS SAM defining the 5 S3 buckets (web, intake, evidence with 7-day Object Lock Governance + SSE-KMS, sanitized, kb-docs), 3 DynamoDB tables (PITR enabled), and SQS DLQ (2-day retention). | 05/11/2026 | 05/11/2026 | AWS SAM Resource Reference |
| 6 | - Configure Amazon Cognito User Pool, App Client, and 3 RBAC groups (REPORTER, SECOPS ADMIN, AUDITOR) in the SAM template.<br>- Provision demo user accounts with real send/receive email addresses for upcoming SES Inbound testing. | 06/11/2026 | 06/11/2026 | Amazon Cognito Developer Guide |

### Week 8 Achievements:

* Completed 100% of core logic libraries (PII Redactor, URL/Domain Analyzer, Deterministic Policy Engine) passing all local unit tests.
* Built the base AWS SAM infrastructure template covering storage, KMS encryption, DLQ error handling, and Cognito RBAC.
* Provisioned real email-backed test accounts across all three user roles.
