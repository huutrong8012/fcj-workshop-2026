---
title: "Playbooks & Bedrock RAG"
date: 2026-11-24
weight: 2
chapter: false
pre: " <b> 5.4.2. </b> "
---

### Step 1: Sync Playbooks & Provision S3 Vectors Knowledge Base

Upload the 5 markdown/text playbooks to the kb-docs bucket, deploy infra/phishguard-kb-prereqs.yaml (which provisions the S3 Vector bucket, 1,024-dimension cosine playbooks index, and KB service role), and run setup_phishguard_kb.py:

```powershell
$kbDocs = $pgOut['KbDocsBucketName']
aws s3 sync .\kb-docs\playbooks\ "s3://$kbDocs/playbooks/" --region $pgRegion --profile $pgProfile
if ($LASTEXITCODE -ne 0) { throw 'Playbook upload failed.' }

$kbUser = Read-Host 'IAM user name running KB setup in lab account'
aws cloudformation deploy --template-file infra/phishguard-kb-prereqs.yaml --stack-name phishguard-kb-prereqs --capabilities CAPABILITY_NAMED_IAM --parameter-overrides "SetupUserName=$kbUser" --region $pgRegion --profile $pgProfile
if ($LASTEXITCODE -ne 0) { throw 'Failed to create KB prerequisites.' }

.\.venv\Scripts\python.exe scripts/setup_phishguard_kb.py --profile $pgProfile
```

| Bedrock & S3 Vectors Setting | Lab Configuration |
| :--- | :--- |
| **Vector Store & Index** | Amazon S3 Vectors (playbooks index) |
| **Embedding Model** | Titan Text Embeddings V2 (float32, 1,024 dimensions, cosine distance) |
| **Data Source & Evidence** | playbooks/ prefix in KbDocsBucketName; latest ingestion job COMPLETE with 5 indexed documents |

### Step 2: Create Bedrock Guardrail & Update Backend Stack

1. Record the KnowledgeBaseId printed after GATE: PHISHGUARD_KB_RETRIEVE_PASS.
2. In **Amazon Bedrock → Guardrails**, create a guardrail enabling **Prompt attack** filtering on user input (BLOCK action), create a numbered version, and confirm **Amazon Nova Lite v1** access in the Model Catalog.
3. Update the backend stack with the Knowledge Base and Guardrail identifiers:
   ```powershell
   $kb = Read-Host 'KnowledgeBaseId'
   $gr = Read-Host 'GuardrailId'
   $gv = Read-Host 'Published GuardrailVersion'
   Update-PgStack @{KnowledgeBaseId=$kb; GuardrailId=$gr; GuardrailVersion=$gv}
   ```

![Amazon Bedrock Knowledge Base Sync History](/images/5-Workshop/5.4-Email-AI/5.4.2-Bedrock/kb-sync-history.png)

### Step 3: Validate Playbook Retrieval Queries

Verify that all 5 playbooks are indexed and test retrieval accuracy using check_playbooks.py or the Bedrock Knowledge Base test console:

```powershell
.\.venv\Scripts\python.exe .\scripts\check_playbooks.py --kb $kb --profile $pgProfile
```

| Test Query | Expected Source Playbook & Interpretation |
| :--- | :--- |
| *Return-Path in EML differs from From. Is there sufficient evidence to conclude the original email failed DMARC?* | **Playbook 04:** Distinguishes outer forwarding envelope verification from untrusted headers inside forwarded attachments. |
| *A HIGH-severity incident lacks a valid domain target to block. What should the analyst do next?* | **Playbook 05:** Reviews indicators and routes the incident to manual SecOps review (NEEDS_REVIEW). |
