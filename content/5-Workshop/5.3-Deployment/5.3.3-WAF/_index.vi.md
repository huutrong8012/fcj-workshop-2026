---
title: "Bảo vệ CloudFront bằng WAF"
date: 2026-11-24
weight: 3
chapter: false
pre: " <b> 5.3.3. </b> "
---

### Bước 1: Triển khai và gắn Web ACL PhishGuardEdge

Chạy template infra/phishguard-waf.yaml ở chế độ quan sát (COUNT COUNT) trước khi bật chặn thực tế:

```powershell
$awsOpts = @('--profile', $pgProfile, '--region', $pgRegion, '--no-cli-pager')
function Set-PgWaf([string]$mode, [string]$probe) {
    aws cloudformation deploy --stack-name phishguard-edge-waf --template-file .\infra\phishguard-waf.yaml --parameter-overrides "ProtectionMode=$mode" "ProbeMode=$probe" RequestsPerMinute=120 --no-fail-on-empty-changeset @awsOpts
    if ($LASTEXITCODE -ne 0) { throw 'Check CloudFormation stack events.' }
}

Set-PgWaf COUNT COUNT
aws cloudformation describe-stacks --stack-name phishguard-edge-waf --query "Stacks[0].Outputs[?OutputKey=='WebACLArn'].OutputValue | [0]" --output text @awsOpts
```

Mở **AWS WAF & Shield → Web ACLs (phạm vi CloudFront / Global) → PhishGuardEdge → Associated AWS resources → Add AWS resources** và chọn CloudFront distribution của portal.

### Bước 2: Kiểm thử luật Probe Header (COUNT → BLOCK) và chuyển sang ENFORCE

1. Ở chế độ COUNT COUNT, gửi hai lệnh GET (cả hai trả về 200) và kiểm tra chỉ số CountedRequests trong **CloudWatch → Metrics → AWS/WAFV2** tại rule PhishGuardProbe:
   ```powershell
   curl.exe -s -o NUL -w "GET: %{http_code}
" "$pgSite/"
   curl.exe -s -o NUL -w "Probe: %{http_code}
" -H "x-phishguard-workshop: verify-edge" "$pgSite/"
   ```
2. Chuyển luật probe sang chế độ chặn (Set-PgWaf COUNT BLOCK) và chạy lại hai lệnh GET. Lượt truy cập thường trả về 200, còn lượt kèm header probe bị chặn với mã **403**.
3. Đăng nhập portal và kiểm tra các luồng hợp lệ (gửi văn bản 202, tải ảnh PNG 201/204, gửi mẫu .eml) không bị chặn nhầm.
4. Chạy Set-PgWaf ENFORCE OFF để bật nhóm luật quản lý tiêu chuẩn (CommonRuleSet) cùng luật giới hạn tốc độ **120 request/phút/IP** trên các route nộp báo cáo, đồng thời tắt probe test. Thử lại cả hai lệnh GET đều trả về 200.

![Biểu đồ CloudWatch WAF BlockedRequests](/images/5-Workshop/5.3-Deployment/5.3.3-WAF/waf-blocked-metrics.png)
