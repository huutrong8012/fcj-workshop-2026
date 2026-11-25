---
title: "End-to-End Demo Tracing"
date: 2026-11-24
weight: 4
chapter: false
pre: " <b> 5.5.4. </b> "
---

### Standard Verification Fixtures (intake/fixtures/)

Using pre-seeded 1-click fixtures ensures repeatable inputs across reviewers and deployments:

![Standard Verification Fixtures on Portal](/images/5-Workshop/5.5-Verification/5.5.4-Demo-Trace/demo-fixtures.png)

| Demo Fixture | Walkthrough Steps | Expected Outcomes & Constraints |
| :--- | :--- | :--- |
| **1. Zalo CEO Fraud (DEMO_1)** | Run as reporter, inspect OCR text and triggered rules, switch to SECOPS_ADMIN, verify target/mode, and click Approve. | 65/100 HIGH, proposes FINANCE_VERIFICATION_ALERT, sends SNS alert, logs audit entry, and terminates as SIMULATED. |
| **2. Banking Lookalike .eml (DEMO_2)** | Inspect defanged IOC (login.techc0mbank-vn[.]com), explain lookalike domain rule, and approve BLOCK_DOMAIN. | 80/100 CRITICAL, proposes BLOCK_DOMAIN (DRY_RUN), records APPROVED_DRY_RUN in internal blocklist. |
| **3. Prompt Injection .eml (DEMO_3)** | Compare rules and score against DEMO_2; verify PROMPT_INJECTION_DETECTED flag, synthesis status, and Synthesize IAM boundaries. | Risk score holds invariant at 80/100 CRITICAL; LLM cannot alter scores or invoke Containment. |

### Vietnamese Diacritics Baseline Check

Submitting a benign internal notice (*"THÔNG BÁO NỘI BỘ: Máy in tầng hai đang được bảo trì..."*) confirms accurate Vietnamese diacritic OCR extraction with 0/100 LOW (NO_ACTION):

![Benign Internal Notice OCR Baseline](/images/5-Workshop/5.5-Verification/5.5.4-Demo-Trace/printer-notice.png)
