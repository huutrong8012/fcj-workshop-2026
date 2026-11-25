---
title: "Week 11 Worklog"
date: 2026-11-23
weight: 11
chapter: false
pre: " <b> 1.11. </b> "
---

### Week 11 Objectives (MVP Phase 3 & Final Delivery):

* **Build Web Portal & Dashboard (React SPA) & Activate AWS WAF:** Develop the frontend application, integrate Cognito JWT authentication, deploy via S3 + CloudFront (OAC), and enable AWS WAF (EnableWaf=true).
* **Integrate 3 One-Click Demo Scenarios & Execute 15 Mandatory Test Cases:** Connect the 3 demo buttons through the real serverless pipeline and validate all 15 functional/security acceptance tests.
* **Operational Monitoring & Workshop Completion:** Configure CloudWatch Alarms/Dashboards, AWS Budgets, align the architecture diagram 100% with deployed resources, and finalize the 7-section Workshop guide.
* **Demo Video, Pitch Rehearsal & Final Submission:** Record the end-to-end product demo video, conduct team pitch rehearsal, and submit all deliverables to the FCAJ Portal.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| 2 | - Build and deploy the React **Reporter Portal** and **SecOps & Auditor Dashboard** to S3 web behind CloudFront (OAC), verifying Authorization header forwarding and enabling **AWS WAF** (Managed Rules + Rate-based rule). | 23/11/2026 | 23/11/2026 | PhishGuard Spec 1.1, 3.2, 5.1 |
| 3 | - Validate the **3 one-click demo scenarios** on the live deployment:<br>  1. Zalo CEO impersonation screenshot → 65, HIGH, proposes FINANCE VERIFICATION ALERT.<br>  2. Spoofed Techcombank email → 80, CRITICAL, proposes BLOCK DOMAIN (DRY RUN).<br>  3. Prompt Injection trap email → score holds invariant at 80, CRITICAL, flags PROMPT INJECTION DETECTED. | 24/11/2026 | 24/11/2026 | PhishGuard Spec 8.1 |
| 4 | - Execute and document evidence for all **15 mandatory test cases** (Section 8.2): duplicate key handling, 409 conflict, Ingest retry without extra evidence versions, unverified SES sender rejection (UNVERIFIED SENDER), benign bank advisory exclusion, blurry image (NEEDS MORE INFO), 60s demo timeout (ESCALATED TIMEOUT), and 403/404 RBAC isolation. | 25/11/2026 | 25/11/2026 | PhishGuard Spec 8.2 |
| 5 | - Configure **CloudWatch Alarms** (SQS DLQ ≥ 1 message, Step Functions FAILED, Lambda errors, Bedrock throttles), **CloudWatch Dashboard**, and **AWS Budgets**.<br>- Audit PhishGuard Architecture v2.2.drawio to ensure 100% parity with deployed services; finalize the **Workshop** guide (all 7 FCAJ sections, Pricing Calculator breakdown, and Clean-up steps). | 26/11/2026 | 26/11/2026 | PhishGuard Spec 9, 10 |
| 6 | - Record the **Product Demo Video** (Full HD with narration), conduct the 5-member team pitch rehearsal, and submit all deliverables (live Production URL, Demo Video, Workshop Guide, Architecture Diagram, and Source Code) to the FCAJ Portal. | 27/11/2026 | 27/11/2026 | FCAJ Portal, AWS Billing Console |

### Week 11 Achievements:

* Delivered the complete **PhishGuard AI** system in production secured with AWS WAF, CloudFront (OAC), Cognito RBAC, Step Functions, Amazon Bedrock (RAG + Guardrails), and S3 Object Lock.
* Passed all 3 one-click demo scenarios and all 15 mandatory test cases on the live AWS environment.
* Operationalized monitoring and cost alerts using CloudWatch Alarms, Dashboards, and AWS Budgets.
* Completed the 7-section **AWS Workshop** guide, recorded the Product Demo Video, and submitted all final deliverables to the FCAJ Portal.
