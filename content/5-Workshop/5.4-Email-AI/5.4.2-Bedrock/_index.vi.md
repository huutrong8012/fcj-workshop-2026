---
title: "Playbook và Bedrock"
date: 2026-11-24
weight: 2
chapter: false
pre: " <b> 5.4.2. </b> "
---

### Bước 1: Đồng bộ 5 Playbook và khởi tạo Knowledge Base trên S3 Vectors

Tải 5 file playbook lên bucket kb-docs, triển khai infra/phishguard-kb-prereqs.yaml (tạo S3 Vector bucket, index playbooks 1.024 chiều cosine và IAM role cho KB), sau đó chạy script setup_phishguard_kb.py:

```powershell
$kbDocs = $pgOut['KbDocsBucketName']
aws s3 sync .\kb-docs\playbooks\ "s3://$kbDocs/playbooks/" --region $pgRegion --profile $pgProfile
if ($LASTEXITCODE -ne 0) { throw 'Upload playbook that bai.' }

$kbUser = Read-Host 'Ten IAM user thuc hien setup KB trong tai khoan lab'
aws cloudformation deploy --template-file infra/phishguard-kb-prereqs.yaml --stack-name phishguard-kb-prereqs --capabilities CAPABILITY_NAMED_IAM --parameter-overrides "SetupUserName=$kbUser" --region $pgRegion --profile $pgProfile
if ($LASTEXITCODE -ne 0) { throw 'Chua tao duoc tai nguyen KB.' }

.\.venv\Scripts\python.exe scripts/setup_phishguard_kb.py --profile $pgProfile
```

| Trường cần đối chiếu ở Bedrock và S3 Vectors | Giá trị cấu hình trong Lab |
| :--- | :--- |
| **Vector store và index** | Amazon S3 Vectors (playbooks) |
| **Mô hình Embedding** | Titan Text Embeddings V2 (float32, 1.024 chiều, cosine) |
| **Nguồn tài liệu và minh chứng** | Prefix playbooks/ trong bucket KB docs; lần sync gần nhất COMPLETE với đủ 5 tài liệu |

### Bước 2: Tạo Bedrock Guardrail và cập nhật Backend Stack

1. Ghi lại KnowledgeBaseId khi script hiển thị GATE: PHISHGUARD_KB_RETRIEVE_PASS.
2. Vào **Amazon Bedrock → Guardrails**, tạo guardrail bật bộ lọc **Prompt attacks** trên input (hành vi chặn BLOCK), phát hành phiên bản mới và xác nhận quyền gọi model **Amazon Nova Lite v1** trong Model catalog.
3. Cập nhật backend với thông số Knowledge Base và Guardrail vừa tạo:
   ```powershell
   $kb = Read-Host 'KnowledgeBaseId'
   $gr = Read-Host 'GuardrailId'
   $gv = Read-Host 'GuardrailVersion da phat hanh'
   Update-PgStack @{KnowledgeBaseId=$kb; GuardrailId=$gr; GuardrailVersion=$gv}
   ```

![Lịch sử đồng bộ Amazon Bedrock Knowledge Base](/images/5-Workshop/5.4-Email-AI/5.4.2-Bedrock/kb-sync-history.png)

### Bước 3: Kiểm tra truy xuất năm Playbook

Kiểm tra 5 tài liệu đã lập chỉ mục thành công và chạy thử truy xuất nguồn bằng script check_playbooks.py:

```powershell
.\.venv\Scripts\python.exe .\scripts\check_playbooks.py --kb $kb --profile $pgProfile
```

| Câu hỏi kiểm thử | Nguồn playbook kỳ vọng và cách đọc |
| :--- | :--- |
| *Return-Path trong EML khác From. Có đủ căn cứ kết luận DMARC của thư gốc thất bại không?* | **Playbook 04:** Phân biệt xác thực của email lớp ngoài với header do người gửi chuyển tiếp cung cấp. |
| *Sự cố HIGH thiếu domain đủ căn cứ để chặn. Chuyên viên nên làm gì tiếp?* | **Playbook 05:** Đối chiếu các chỉ dấu và chuyển sang nhánh kiểm tra thủ công (NEEDS_REVIEW). |
