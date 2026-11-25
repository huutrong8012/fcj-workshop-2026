---
title: "Week 3 Worklog"
date: 2026-09-28
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---

### Week 3 Objectives:

* **Workflow Orchestration with AWS Step Functions:** Master Standard Workflows, Choice states, Retry/Catch error handling, and the .waitForTaskToken callback pattern.
* **Infrastructure as Code (IaC) with AWS SAM:** Adopt AWS Serverless Application Model (SAM CLI) to define cloud infrastructure declaratively.
* **Specialized AI Services Exploration:** Investigate Amazon Textract (OCR), Amazon Bedrock (Converse API, Knowledge Bases, Guardrails), and Amazon S3 Vectors.
* **Data & Digital Evidence Protection:** Study SSE-KMS, S3 Versioning, and S3 Object Lock (Governance Mode) for forensic readiness.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| 2 | - Study AWS Step Functions Standard Workflows.<br>- Build a prototype State Machine utilizing Choice branching, Retry/Catch blocks, and lambda:invoke.waitForTaskToken with SendTaskSuccess for human approval pauses. | 28/09/2026 | 28/09/2026 | AWS Step Functions Developer Guide |
| 3 | - Install and practice with AWS SAM CLI (sam init, sam build, sam deploy).<br>- Learn YAML template structuring, environment parameterization, and unified serverless resource management. | 29/09/2026 | 29/09/2026 | AWS SAM Documentation, AWS Cloud & AI Day Watch Party |
| 4 | - Explore Amazon Bedrock Converse API, structured JSON output enforcement, and Bedrock Guardrails (filtering harmful content and prompt attacks via guardContent).<br>- Study Bedrock Knowledge Bases using Titan Text Embeddings V2 paired with **Amazon S3 Vectors** to optimize costs over OpenSearch Serverless. | 30/09/2026 | 30/09/2026 | Amazon Bedrock User Guide, S3 Vectors Documentation |
| 5 | - Test Amazon Textract DetectDocumentText API for extracting text from screenshots.<br>- Research secure digital evidence storage on Amazon S3: SSE-KMS (CMK), Versioning, and Object Lock Governance mode. | 01/10/2026 | 01/10/2026 | Amazon Textract & Amazon S3 Object Lock Docs |
| 6 | - Conduct weekly team sync to consolidate technical findings and map them to **PhishGuard AI** requirements.<br>- Analyze multi-channel ingestion paths: Web Portal (Presigned POST) and Email Forwarding (Amazon SES Inbound Receipt Rules). | 02/10/2026 | 02/10/2026 | Amazon SES Developer Guide, Team Technical Sync |

### Week 3 Achievements:

* Mastered workflow pause-and-resume orchestration using Step Functions .waitForTaskToken—the core backbone for PhishGuard AI's Human-in-the-Loop approval gate.
* Gained hands-on proficiency in AWS SAM for repeatable infrastructure deployment and clean resource teardown.
* Selected Amazon S3 Vectors as a cost-effective vector store for the Bedrock Knowledge Base instead of OpenSearch Serverless.
* Understood forensic readiness practices on S3 using Versioning, Object Lock Governance, and SSE-KMS encryption.
