---
title: "Acceptance Suite H01–H15"
date: 2026-11-24
weight: 2
chapter: false
pre: " <b> 5.5.2. </b> "
---

### Automated Verification Scripts

Execute the local and live acceptance verification scripts from lab/:

```powershell
.\.venv\Scripts\python.exe scripts/check_phase_h_local.py
.\.venv\Scripts\python.exe scripts/check_phase_h_live.py --profile $pgProfile --other-reporter demo-reporter-2
.\.venv\Scripts\python.exe scripts/verify_group1_upload.py --profile $pgProfile
```

![Terminal Output for H01–H03 Idempotency and Isolation](/images/5-Workshop/5.5-Verification/5.5.2-Acceptance/h01-h03-terminal.png)

### Acceptance Test Matrix (H01–H15)

| Case | Expected Verification Result |
| :--- | :--- |
| **H01** | Same reporter, key, and payload return the identical incident_id (INC-8066dcaaa5e4) and single named Step Functions execution. |
| **H02** | Two reporters submitting identical keys/content produce two distinct incidents; cross-user reads are denied (403/404). |
| **H03** | Reusing an idempotency key with different content returns HTTP 409; malformed key returns HTTP 400. |
| **H04** | Simulated interruption after evidence write preserves a single S3 evidence version and single workflow start upon retry. |
| **H05** | .eml with Return-Path differing from From records INDETERMINATE (EMBEDDED_UNVERIFIED) without false AUTH_DMARC_FAIL. |
| **H06** | Inbound SES email from an unverified sender logs UNVERIFIED_SENDER, increments UnverifiedSender metric, and creates no incident. |
| **H07** | Benign bank advisory warning users never to share OTP scores 0 points (NO_ACTION). |
| **H08** | Advisory text paired with a phishing OTP URL triggers FINANCIAL_OR_OTP_REQUEST (+30). |
| **H09** | Blank image routes to NEEDS_MORE_INFO with risk score left blank. |
| **H10** | Comparing DEMO_2 and DEMO_3: sample 3 flags PROMPT_INJECTION_DETECTED while keeping score invariant at 80/100. |
| **H11** | HIGH-severity report without a valid blockable target routes to NEEDS_REVIEW and triggers an SNS alert. |
| **H12** | All 5 hostname cases in the local unit test suite pass brand lookalike domain detection accurately. |
| **H13** | Duplicate approval submissions with the same decision_id execute idempotently with a single audit log and callback. |
| **H14** | Approval timeout transitions to ESCALATED_TIMEOUT; subsequent late decision attempts return HTTP 409. |
| **H15** | REPORTER and AUDITOR responses and SNS payloads never leak sfn_task_token, raw_s3_key, or raw email headers. |

### Key Evidence Captures (H05, H11, H14)

1. **H05 Forwarded Header Handling & NO_ACTION Step Functions Path:**
   ![H05 Email Auth Indeterminate](/images/5-Workshop/5.5-Verification/5.5.2-Acceptance/h05-indeterminate.png)
   ![Step Functions MarkNoAction Execution](/images/5-Workshop/5.5-Verification/5.5.2-Acceptance/sfn-no-action.png)
2. **H11 Manual Review Escalation (NEEDS_REVIEW):** Upload fixtures/phase-h/H11-manual-review.eml via Presigned POST; verify the dashboard shows 50/100 HIGH (NEEDS_REVIEW) with triggered rules and RAG playbook citations:
   ![H11 Upload EML via Presigned POST](/images/5-Workshop/5.5-Verification/5.5.2-Acceptance/h11-upload.png)
   ![H11 Incident Row on Dashboard](/images/5-Workshop/5.5-Verification/5.5.2-Acceptance/h11-row.png)
   ![H11 Policy Engine Rules and RAG Citations](/images/5-Workshop/5.5-Verification/5.5.2-Acceptance/h11-detail.png)
3. **H14 Approval Timeout & Late Decision Rejection (409):** Temporarily set ApprovalTimeoutSeconds=60, allow a DEMO_1 incident to expire (ESCALATED_TIMEOUT), run check_phase_h_live.py --expired <incident-id> to confirm H14_LATE_DECISION_409: PASS, and restore timeout to 172800:
   ![H14 Expired Incident Row](/images/5-Workshop/5.5-Verification/5.5.2-Acceptance/h14-timeout-row.png)
   ![H14 Late Decision 409 Terminal Output](/images/5-Workshop/5.5-Verification/5.5.2-Acceptance/h14-409-terminal.png)
