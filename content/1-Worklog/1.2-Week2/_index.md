---
title: "Week 2 Worklog"
date: 2026-09-21
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---

### Week 2 Objectives:

* **Security & Networking Foundations (IAM & Networking):** Master the Principle of Least Privilege in IAM and secure network design principles.
* **Serverless & Event-Driven Architecture:** Deep dive into event-driven patterns using AWS Lambda, Amazon S3 Events, Amazon DynamoDB, and API Gateway.
* **Capstone Project Kickoff (Ideation):** Analyze multi-channel phishing challenges in Vietnamese enterprises and propose the **PhishGuard AI** concept.
* **Initial Scoping & Review:** Define project scope and justify combining a Deterministic Policy Engine with Generative AI in a Human-in-the-Loop SOAR model.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| 2 | - Conduct advanced hands-on labs on AWS IAM: IAM Roles, Permission Policies, Trust Policies, and IAM Policy Simulator.<br>- Study the Principle of Least Privilege and Separation of Duties in multi-service architectures. | 21/09/2026 | 21/09/2026 | AWS IAM User Guide, AWS Security Best Practices |
| 3 | - Explore Serverless & Event-Driven Architecture patterns in depth.<br>- Build a lab connecting Amazon S3 Event Notifications to trigger AWS Lambda and persist records in Amazon DynamoDB (On-Demand, GSI, TTL). | 22/09/2026 | 22/09/2026 | AWS Serverless Workshop, DynamoDB Developer Guide |
| 4 | - Study Amazon API Gateway (HTTP API) and JWT authentication using Amazon Cognito User Pools.<br>- Configure Cognito User Groups to enforce Role-Based Access Control (RBAC). | 23/09/2026 | 23/09/2026 | Amazon Cognito & API Gateway Documentation |
| 5 | - Hold team ideation session on real-world phishing threats: spoofed bank/partner emails and CEO impersonation on Zalo/WhatsApp requesting urgent wire transfers or OTPs.<br>- Analyze risks of autonomous AI blocking (PII leakage, Prompt Injection, False Positives) versus SecOps manual triage overload. | 24/09/2026 | 24/09/2026 | Vietnam Cybersecurity Reports, OWASP Top 10 for LLM |
| 6 | - Finalize project title: **PhishGuard AI** – Multi-Channel Phishing Triage & Human-in-the-Loop SOAR Copilot on AWS Serverless.<br>- Draft Project Proposal v1.0 and assign technical research tracks across the 5 team members. | 25/09/2026 | 25/09/2026 | FCAJ Project Proposal Template, Team Brainstorming |

### Week 2 Achievements:

* Mastered fine-grained IAM policy authoring and JWT-based RBAC authentication via Amazon Cognito.
* Gained solid proficiency in DynamoDB data modeling using Global Secondary Indexes (GSI) and TTL attributes.
* Defined the core value proposition of **PhishGuard AI**: pairing a transparent, injection-proof Deterministic Policy Engine with Amazon Bedrock (RAG & Guardrails) and mandatory human approval before executing containment actions.
* Completed Project Proposal v1.0 ready for FCAJ Mentor review.
