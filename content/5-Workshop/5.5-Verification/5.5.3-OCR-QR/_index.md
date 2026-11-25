---
title: "OCR & QR Decoding Tests"
date: 2026-11-24
weight: 3
chapter: false
pre: " <b> 5.5.3. </b> "
---

Log in as a lab reporter via CloudFront and upload each test image from lab/fixtures/qr/ sequentially, followed by running DEMO_1 to verify combined OCR and QR extraction:

| Submission Order | Demo Run Outcome | Detail Inspection Notes |
| :--- | :--- | :--- |
| **1. blank.png** | NEEDS_MORE_INFO (unscored) | Contains neither readable OCR text nor a valid QR URL. |
| **2. qr-url.png** | 0/100 LOW (NO_ACTION) | Extracts canonical IOC hxxps://example[.]org/workshop/qr with query and fragment stripped. With a clean blocklist, a valid URL alone adds 0 points. |
| **3. qr-text.png** | NEEDS_MORE_INFO (unscored) | QR payload is plain text rather than an HTTP(S) URL; extractor only promotes valid HTTP(S) URLs to IOCs. |
| **4. DEMO_1 sample image** | 65/100 HIGH (PENDING_APPROVAL) | Extracts Vietnamese OCR text and triggers FINANCIAL_OR_OTP_REQUEST, EXEC_IMPERSONATION, and URGENCY_PRESSURE. |

![Dashboard Verification of OCR and QR Image Submissions](/images/5-Workshop/5.5-Verification/5.5.3-OCR-QR/ocr-qr-rows.png)
