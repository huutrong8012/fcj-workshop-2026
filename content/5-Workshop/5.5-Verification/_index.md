---
title: "Deployment Verification"
date: 2026-11-24
weight: 5
chapter: false
pre: " <b> 5.5. </b> "
---

Run verification checks against the live lab deployment and record timestamps, incident IDs, and evidence screenshots for every test group.

| Sub-section | Expected Outcome |
| :--- | :--- |
| **[5.5.1. Resources & Monitoring](5.5.1-monitoring/)** | Audit deployed stack settings, CloudWatch alarms, and manual console checkpoints. |
| **[5.5.2. Acceptance Suite H01–H15](5.5.2-acceptance/)** | Validate ownership isolation, idempotency, approvals, and late-decision rejections (409). |
| **[5.5.3. OCR & QR Decoding Tests](5.5.3-ocr-qr/)** | Test blank images, QR URL/text payloads, and Vietnamese OCR baseline fixtures. |
| **[5.5.4. End-to-End Demo Tracing](5.5.4-demo-trace/)** | Trace a single incident ID across the portal, Step Functions, S3 versioned evidence, and audit logs. |
