---
title: "Workshop"
date: 2026-11-24
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# PhishGuard AI Workshop — Phishing Triage and Human-Approved Response on AWS

An employee forwards a suspicious email to the security team, while a colleague uploads a screenshot from the same campaign. SecOps needs to preserve the immutable original artifact for every report, control who can view sensitive evidence, and trace the exact reasoning behind every action. This workshop builds and validates the complete serverless workflow in **US East (N. Virginia) (us-east-1)**.

### Expected Outcome

Submit text, an image, or a forwarded email; trace a single incident ID to its versioned S3 evidence object; and verify the simulated approval outcome (DRY_RUN) or terminal triage branch. When building from scratch, follow the sections in order. If auditing an existing demo deployment, start at **Section 5.5.1** to compare configurations and revisit any section with discrepancies.

| Section | Expected Deliverable |
| :--- | :--- |
| **[5.1. Problem & Architecture](5.1-overview/)** | Trace the end-to-end data flow of a report and locate where raw evidence is preserved. |
| **[5.2. Environment Preparation](5.2-prerequisite/)** | Configure the AWS lab account, IAM permissions, CLI tools, and workshop lab package. |
| **[5.3. Application Deployment](5.3-deployment/)** | Deploy the SAM backend, Cognito/CloudFront portal, and edge AWS WAF protection. |
| **[5.4. Email & AI Integration](5.4-email-ai/)** | Connect Amazon SES inbound email, index 5 playbooks in Bedrock Knowledge Base, and test RAG retrieval. |
| **[5.5. Deployment Verification](5.5-verification/)** | Validate deployed resources, H01–H15 acceptance tests, OCR/QR extraction, and end-to-end demo tracing. |
| **[5.6. Well-Architected Review](5.6-well-architected/)** | Evaluate architectural and operational decisions against the six AWS Well-Architected pillars. |
| **[5.7. Cost Estimate & Actuals](5.7-cost/)** | Reproduce the monthly AWS Pricing Calculator estimate and audit Cost Explorer reports. |
| **[5.8. Resource Cleanup](5.8-cleanup/)** | Tear down billable resources safely in dependency order. |
