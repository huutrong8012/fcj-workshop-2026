---
title: "Week 5 Worklog"
date: 2026-10-12
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---

### Week 5 Objectives:

* **Finalize Engineering Specification (v2.0 → v2.1):** Specify deterministic phishing detection rules (Policy Engine), URL defanging, lookalike domain detection, and structured PII redaction rules.
* **AWS Well-Architected Framework Mapping:** Evaluate the project architecture against all 6 Well-Architected pillars (focusing on Security, Reliability, and Cost Optimization).
* **Author RAG Knowledge Base Corpus:** Draft SecOps incident response playbooks, financial transfer verification policies, and Vietnamese phishing pattern guides.
* **Design Demo Scenarios & QA Test Suite:** Define 3 one-click demo scenarios and 15 mandatory functional/security test cases.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| 2 | - Specify the deterministic risk scoring rules (KNOWN BLOCKED IOC +50, BRAND LOOKALIKE DOMAIN +40, FINANCIAL OR OTP REQUEST +30, EXEC IMPERSONATION +25, URGENCY PRESSURE +10, HEADER ANOMALY +10).<br>- Design the PROMPT INJECTION DETECTED security flag (zero score impact) to prove deterministic score invariance against injection attacks. | 12/10/2026 | 12/10/2026 | MITRE ATT&CK, PhishGuard Technical Spec |
| 3 | - Design structured PII redaction rules (Citizen ID/CCCD, Vietnamese phone numbers, bank account numbers, OTPs, email local-parts) while preserving entity relationships (BANK ACCOUNT 1, EMAIL USER 1) so AI retains context without seeing raw PII.<br>- Define IOC canonicalization and defanging standards (hxxps://...[.]com). | 13/10/2026 | 13/10/2026 | NIST PII Confidentiality Guide |
| 4 | - Author curated documents for the Bedrock Knowledge Base (kb-docs): SecOps Playbook, Out-of-Band Financial Verification Policy, and Common Phishing Scenarios in Vietnam.<br>- Prepare initial SEED records for the PhishGuard Blocklist table. | 14/10/2026 | 14/10/2026 | SOC & Banking Compliance Guidelines |
| 5 | - Map system design to the AWS Well-Architected Framework (Operational Excellence, Security, Reliability, Performance Efficiency, Cost Optimization, Sustainability).<br>- Estimate monthly costs via AWS Pricing Calculator assuming 300 incidents/month and compare S3 Vectors against OpenSearch Serverless. | 15/10/2026 | 15/10/2026 | AWS Well-Architected Framework, AWS Pricing Calculator |
| 6 | - Define 3 one-click demo scenarios (Zalo CEO impersonation screenshot, spoofed Techcombank email, and Prompt Injection trap email) alongside 15 mandatory acceptance test cases.<br>- Compile Technical Specification v2.1. | 16/10/2026 | 16/10/2026 | Team Review & QA Planning |

### Week 5 Achievements:

* Finalized transparent deterministic scoring logic (0–100 scale) and relationship-preserving PII redaction rules.
* Completed the domain-specific RAG corpus (kb-docs) for Amazon Bedrock Knowledge Bases.
* Produced the Well-Architected assessment and AWS Pricing Calculator estimate, validating significant cost savings using S3 Vectors, DynamoDB On-Demand, and intake lifecycle expiration.
* Established the 3 one-click demo scenarios and 15 mandatory test cases as acceptance criteria for implementation.
