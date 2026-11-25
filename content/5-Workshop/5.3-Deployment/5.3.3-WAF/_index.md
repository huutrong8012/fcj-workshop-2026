---
title: "Edge Protection with WAF"
date: 2026-11-24
weight: 3
chapter: false
pre: " <b> 5.3.3. </b> "
---

### Step 1: Deploy and Associate the PhishGuardEdge Web ACL

Deploy the edge WAF CloudFormation stack (infra/phishguard-waf.yaml) initially in COUNT mode:

```powershell
$awsOpts = @('--profile', $pgProfile, '--region', $pgRegion, '--no-cli-pager')
function Set-PgWaf([string]$mode, [string]$probe) {
    aws cloudformation deploy --stack-name phishguard-edge-waf --template-file .\infra\phishguard-waf.yaml --parameter-overrides "ProtectionMode=$mode" "ProbeMode=$probe" RequestsPerMinute=120 --no-fail-on-empty-changeset @awsOpts
    if ($LASTEXITCODE -ne 0) { throw 'Check CloudFormation stack events.' }
}

Set-PgWaf COUNT COUNT
aws cloudformation describe-stacks --stack-name phishguard-edge-waf --query "Stacks[0].Outputs[?OutputKey=='WebACLArn'].OutputValue | [0]" --output text @awsOpts
```

In the AWS Console, navigate to **WAF & Shield → Web ACLs (CloudFront / Global) → PhishGuardEdge → Associated AWS resources → Add AWS resources** and attach your CloudFront distribution.

### Step 2: Validate Header Probe Rule (COUNT → BLOCK) and Switch to ENFORCE

1. With COUNT COUNT active, send two GET requests (both return 200) and verify CountedRequests in **CloudWatch → Metrics → AWS/WAFV2** for rule PhishGuardProbe:
   ```powershell
   curl.exe -s -o NUL -w "GET: %{http_code}
" "$pgSite/"
   curl.exe -s -o NUL -w "Probe: %{http_code}
" -H "x-phishguard-workshop: verify-edge" "$pgSite/"
   ```
2. Switch the probe rule to blocking (Set-PgWaf COUNT BLOCK) and re-run both requests. The standard request returns 200, while the header probe request is blocked with HTTP **403**.
3. Log in to the web portal and verify legitimate submissions (text 202, PNG presign 201 + S3 POST 204, .eml fixture) succeed without false positives.
4. Activate full enforcement with the probe disabled (Set-PgWaf ENFORCE OFF). Managed rule groups enforce their configured actions, and the rate-based rule enforces **120 requests/minute per IP** on submission routes. Re-test both GET requests to confirm both return 200.

![CloudWatch WAF BlockedRequests Metric](/images/5-Workshop/5.3-Deployment/5.3.3-WAF/waf-blocked-metrics.png)
