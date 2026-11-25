---
title: "Week 7 Worklog"
date: 2026-10-26
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---

### Week 7 Objectives:

* **UI/UX Wireframing:** Design the React SPA interfaces for the Employee Reporter Portal and the SecOps & Auditor Triage Dashboard.
* **API Contract Standardization:** Define exact JSON Request/Response payloads and HTTP status codes (400, 403, 409, 202) across all API endpoints.
* **Repository Scaffolding & Coding Standards:** Set up the Git repository, AWS SAM folder structure, LLM JSON output schemas, and test datasets (fixtures).
* **Pre-Deployment Foundation:** Configure custom domain DNS records and enable model access on Amazon Bedrock in us-east-1.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| 2 | - Design Web Portal wireframes: Cognito login flow, incident submission form (paste text, upload .eml, upload screenshot), and the **3 one-click demo scenario buttons**. | 26/10/2026 | 26/10/2026 | Figma, React SPA Design Guidelines |
| 3 | - Design Dashboard wireframes for SECOPS ADMIN and AUDITOR: incident queue filtered by status (PENDING APPROVAL, NEEDS REVIEW), PII-redacted evidence table, RAG citations, 60-second raw evidence viewer button, and APPROVE/REJECT decision controls. | 27/10/2026 | 27/10/2026 | SecOps Dashboard UX Patterns |
| 4 | - Document API Contracts for all endpoints: POST /api/uploads/presign, POST /api/reports, GET /api/incidents, GET /api/incidents/{id}, GET /api/incidents/{id}/raw-evidence-url, POST /api/incidents/{id}/decision, GET /api/audit-logs, and GET /api/blocklist. | 28/10/2026 | 28/10/2026 | OpenAPI 3.0 Specification |
| 5 | - Initialize modular Git repository: infra/ (AWS SAM templates), backend/ (8 Lambda functions + shared core), frontend/ (React SPA), kb-docs/ (RAG corpus), and fixtures/ (3 demo scenarios + 5 Vietnamese Zalo screenshots for OCR benchmarking). | 29/10/2026 | 29/10/2026 | AWS SAM Best Practices |
| 6 | - Configure custom domain DNS, add MX records, and verify Domain Identity on Amazon SES in us-east-1.<br>- Enable Model Access for **Amazon Nova Lite** and **Titan Text Embeddings V2** in Amazon Bedrock. | 30/10/2026 | 30/10/2026 | Amazon SES & Bedrock Console |

### Week 7 Achievements:

* Completed UI/UX wireframes covering workflows for all 3 RBAC roles (REPORTER, SECOPS ADMIN, AUDITOR).
* Finalized API Contracts enabling parallel Backend and Frontend development without schema mismatches.
* Established a clean repository structure along with realistic test fixtures (Vietnamese Zalo screenshots, spoofed bank emails, and Prompt Injection payloads).
* Verified custom domain on Amazon SES and enabled Bedrock foundation models in us-east-1.
