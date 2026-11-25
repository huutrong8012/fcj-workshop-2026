---
title: "Sharing and Feedback"
date: 2026-12-06
weight: 7
chapter: false
pre: " <b> 7. </b> "
---

Participating in the **AWS First Cloud AI Journey (FCAJ)** program has been a rewarding milestone in my academic and technical journey. Below are my honest reflections and constructive feedback based on my 12-week experience building **PhishGuard AI** with my team:

### Overall Evaluation

**1. Working Environment**  
The learning and working environment at FCAJ—especially during sessions and weekly meetups at Bitexco Financial Tower—is energetic, welcoming, and collaborative. Fellow builders and program coordinators are always willing to share insights and troubleshoot issues even outside regular syncs. Adding a few informal cross-team networking or bonding sessions early in the cohort would make it even easier for interns from different groups to connect.

**2. Support from Mentor / Team Admin**  
The mentors provided sharp, architecture-level feedback rather than handing out ready-made solutions. During our **PhishGuard AI** design reviews, mentor critiques helped us uncover critical edge cases—such as forwarded email DMARC validation, idempotency collisions, and compensating controls for direct S3 Presigned POST uploads—pushing us to think like production cloud security engineers. The admin team also maintained clear documentation templates, portals, and responsive logistical support throughout the program.

**3. Relevance of Work to Academic Major**  
As an Information Security student, building a Human-in-the-Loop Phishing Triage & SOAR Copilot on AWS Serverless aligned directly with my major. It allowed me to apply core security principles—Least Privilege IAM, RBAC, cryptographic evidence integrity (S3 Object Lock & KMS), PII redaction, and AI Guardrails against Prompt Injection—to a practical, end-to-end cloud architecture.

**4. Learning & Skill Development Opportunities**  
The program accelerated both my technical depth and engineering workflow. Beyond hands-on mastery of AWS SAM, Step Functions, Amazon Bedrock (RAG & Guardrails), Textract, and CloudFront/WAF, I learned how to write rigorous technical specifications, estimate cloud costs via AWS Pricing Calculator, design acceptance test suites, and collaborate effectively in a 5-member engineering team.

**5. Community Culture & Team Spirit**  
FCAJ fosters a strong builder culture where knowledge sharing is genuine and constructive. Whether during Saturday meetups, architecture reviews, or late-sprint debugging with teammates, everyone supported one another with mutual respect and shared enthusiasm for Cloud and AI.

**6. Program Resources & Benefits**  
The $200 AWS Credits lab roadmap gave us a structured, low-risk way to get hands-on with core services before architecting our capstone project. Additionally, complimentary access to flagship community events—such as *AWS Cloud & AI Day Hanoi 2026*, *Fireside Chat with Dr. Werner Vogels*, *Buildrathon Kickoff*, and *FCAJ Weekly Meetups*—was an invaluable highlight of the program.

---

### Additional Questions

- **What did you find most satisfying during your internship?**  
  Seeing **PhishGuard AI** evolve from an initial whiteboard idea into a live, end-to-end serverless security platform where deterministic rules, PII redaction, Bedrock RAG/Guardrails, and human-in-the-loop Step Functions approvals worked seamlessly together—and validating that our risk scoring remained invariant even against real Prompt Injection payloads.

- **What do you think the program should improve for future interns?**  
  Providing a dedicated sandbox environment or pre-verified test domain guidelines earlier in the program would help teams testing email-driven services (such as Amazon SES Inbound) avoid DNS propagation bottlenecks and account sandbox limits.

- **If recommending to a friend, would you suggest they intern here? Why or why not?**  
  Definitely yes. FCAJ bridges the gap between university theory and real-world AWS cloud engineering. You are challenged to design, cost-optimize, secure, and deploy a complete production-style system from scratch while learning directly from experienced AWS practitioners and community leaders.

---

### Suggestions & Expectations

- **Do you have any suggestions to improve the internship experience?**  
  Hosting a mid-program "Architecture & Security Clinic" where teams can do quick 15-minute peer reviews of each other's IAM policies, cost estimates, and edge cases before the final MVP sprint would add great value.

- **Would you like to continue this program in the future?**  
  Yes, I would love to stay involved with the FCAJ community—both to deepen my specialization in Cloud Security & AI Security and to return in future cohorts as a supporter or mentor for incoming students.

- **Any other comments (free sharing):**  
  Thank you to the entire FCAJ organizing team, mentors, guest speakers, and my four teammates for an unforgettable 12-week journey. The engineering habits, security mindset, and community connections I gained here will stay with me throughout my career.
