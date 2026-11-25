---
title: "Resource Cleanup"
date: 2026-11-24
weight: 8
chapter: false
pre: " <b> 5.8. </b> "
---

Follow the dependency order below when tearing down the lab environment after grading is complete:

1. **Retain Until Grading Completes:** Keep the production URL active while evaluators review the live demo. Export all acceptance test evidence, Pricing Calculator PDFs, and Billing CSV reports beforehand.
2. **Stop Inbound Ingestion & Clean SES:** Pause new submissions, check for any open PENDING_APPROVAL executions in Step Functions, and remove the lab receipt rule from Amazon SES (preserving MX and email rules if sharing a domain).
3. **Disassociate & Delete CloudFront and WAF:** Disassociate the PhishGuardEdge Web ACL from your CloudFront distribution, disable the distribution, wait for Deployed status, and delete it. Then delete the phishguard-edge-waf stack and unsubscribe demo emails from SNS.
4. **Delete Bedrock Knowledge Base & S3 Vectors:** Delete the Bedrock data source and Knowledge Base first, followed by the phishguard-kb-prereqs stack and S3 Vector index/bucket.
5. **Empty S3 Buckets & Delete Main SAM Stack:** Empty non-locked S3 buckets (web, intake, sanitized, kb-docs). For the evidence bucket with Object Lock Governance, retain objects until the 7-day retention expires or use an authorized governance-bypass role to remove object versions and delete markers before deleting the stack.
6. **Schedule KMS Key Deletion & Audit Billing:** Delete phishguard-ai via CloudFormation, verify all CloudWatch log groups/alarms, SQS queues, Cognito domains, and Budgets are removed, schedule KMS Customer Managed Key deletion (7–30 day waiting period), and re-check **AWS Cost Explorer** after 24 hours.
