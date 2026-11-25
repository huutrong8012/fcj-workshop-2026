---
title: "Application Deployment"
date: 2026-11-24
weight: 3
chapter: false
pre: " <b> 5.3. </b> "
---

Deploy the backend stack first to obtain stack outputs for the React frontend, configure Amazon Cognito and CloudFront origins, and finally attach the edge AWS WAF Web ACL.

| Sub-section | Expected Outcome |
| :--- | :--- |
| **[5.3.1. Backend & Image Processing](5.3.1-backend/)** | Deploy the AWS SAM stack and verify the OCR and QR Lambda layers. |
| **[5.3.2. Portal & Access Control](5.3.2-portal/)** | Configure Cognito hosted domain, two CloudFront origins, and deploy the live React SPA. |
| **[5.3.3. Edge Protection with WAF](5.3.3-waf/)** | Attach PhishGuardEdge Web ACL, verify custom header blocking (403), and run portal smoke tests. |
