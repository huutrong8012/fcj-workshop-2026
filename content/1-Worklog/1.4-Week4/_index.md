---
title: "Week 4 Worklog"
date: 2026-10-05
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---

### Week 4 Objectives:

* **Edge Architecture & Web Security:** Study Amazon CloudFront, Origin Access Control (OAC), and AWS WAF (Managed Rules & Rate-based Rules).
* **End-to-End Architecture Design (PhishGuard AI v1.0):** Draft the 11-step architecture diagram covering multi-channel ingestion through simulated containment and audit logging.
* **Data Modeling & Access Control Design:** Define schemas for 3 DynamoDB tables, 5 S3 buckets, and a Least Privilege IAM matrix for 8 Lambda functions.
* **Mentor Architecture Review:** Present Architecture v1.0 to the FCAJ Mentor to identify security edge cases and validate feasibility.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| 2 | - Study Amazon CloudFront distribution setup serving a React SPA from S3 (via OAC) and routing /api/* to API Gateway HTTP API.<br>- Explore AWS WAF Managed Rules, Rate-based Rules, and Origin Request Policies to forward the Authorization header. | 05/10/2026 | 05/10/2026 | Amazon CloudFront & AWS WAF Developer Guides |
| 3 | - Create the **PhishGuard AI** architecture diagram in Draw.io illustrating all 11 end-to-end processing steps.<br>- Design strict isolation zones between staging (intake), raw evidence (evidence), and PII-redacted data (sanitized). | 06/10/2026 | 06/10/2026 | AWS Architecture Icons, Draw.io |
| 4 | - Design schemas for 3 DynamoDB tables (PhishGuard Incidents, PhishGuard AuditLogs, PhishGuard Blocklist) including partition keys, GSIs (ReporterIndex, StatusIndex), and append-only IAM constraints for AuditLogs.<br>- Define the incident state machine lifecycle from UPLOAD PENDING to terminal states. | 07/10/2026 | 07/10/2026 | Amazon DynamoDB Best Practices |
| 5 | - Construct the Least Privilege IAM matrix for 8 Lambda functions (Reporter API, Admin API, Ingest, Extract & Redact, Triage, Synthesize, PrepareApproval, Containment) and 3 Cognito RBAC roles (REPORTER, SECOPS ADMIN, AUDITOR). | 08/10/2026 | 08/10/2026 | AWS Well-Architected Security Pillar |
| 6 | - Present **PhishGuard AI Architecture v1.0** to the FCAJ Mentor.<br>- Document critical review feedback regarding idempotency enforcement, SES forwarded email authentication, and compensating controls for direct S3 Presigned POST uploads. | 09/10/2026 | 09/10/2026 | FCAJ Architecture Review Session |

### Week 4 Achievements:

* Completed Architecture Diagram v1.0 for **PhishGuard AI** covering all 11 serverless processing stages.
* Designed a multi-tiered storage model (5 S3 buckets + 1 S3 Vector bucket + 3 DynamoDB tables) ensuring strict isolation of sensitive PII and raw evidence.
* Established a 3-role RBAC model and an 8-Lambda Least Privilege IAM matrix preventing unauthorized access to raw evidence or autonomous containment execution.
* Gathered actionable technical feedback from the Mentor to refine the engineering specification.
