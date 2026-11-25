---
title: "Inbound Email via SES"
date: 2026-11-24
weight: 1
chapter: false
pre: " <b> 5.4.1. </b> "
---

### Step 1: Verify Domain Identity & Configure MX Record

1. In **Amazon SES (us-east-1)**, create a Domain Identity for your domain or lab subdomain, publish the Easy DKIM CNAME records in your DNS provider, and wait until **DKIM configuration** shows **Successful / Verified**.
2. Add an MX record with priority 10 pointing to inbound-smtp.us-east-1.amazonaws.com for the receiving domain/subdomain.

![Amazon SES DKIM Verification Successful](/images/5-Workshop/5.4-Email-AI/5.4.1-SES/ses-dkim.png)

### Step 2: Configure the SES Receipt Rule

1. Open **Amazon SES → Email receiving → Receipt rule sets** and select your active rule set.
2. Create an enabled receipt rule targeting your SOC recipient address (e.g., soc@<your-domain>) with two sequential actions:
   - **Action 1 — Deliver to S3:** Select the stack's intake bucket and set the object key prefix to inbound/.
   - **Action 2 — Invoke AWS Lambda function:** Select PhishGuard-Ingest with invocation type **Event (asynchronous)**.

![Amazon SES Receipt Rule Lambda Action](/images/5-Workshop/5.4-Email-AI/5.4.1-SES/ses-lambda-action.png)

### Step 3: End-to-End Email Forwarding Verification

From a verified Cognito reporter mailbox belonging to AllowedSenderDomains, forward a benign email to soc@<your-domain>. Confirm a new MIME object appears in s3://<intake-bucket>/inbound/ and the corresponding incident appears on the dashboard with source_channel = SES_EMAIL and reporter_identity_status = VERIFIED_SES_DMARC.
