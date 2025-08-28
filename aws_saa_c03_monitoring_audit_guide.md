# AWS Monitoring & Audit Mastery Guide (SAA-C03)

## Overview

Monitoring and auditing in AWS is critical for security, compliance, and operational excellence. Three main services are tested in the AWS Certified Solutions Architect – Associate exam:

- **Amazon CloudWatch** – Monitoring, metrics, alarms, and logs.
- **AWS CloudTrail** – Governance, compliance, and auditing of API calls.
- **AWS Config** – Configuration compliance, resource inventory, and change tracking.

---

## Amazon CloudWatch

### Key Features

- Collects metrics (CPU, memory, custom metrics).
- CloudWatch Alarms trigger actions (SNS, Auto Scaling, Lambda).
- CloudWatch Logs store and query logs (with Insights).
- CloudWatch Dashboards provide visualization.
- CloudWatch Events/EventBridge integrate with Lambda, Step Functions, ECS, etc.

### Exam Architectures

- EC2 → CloudWatch Agent → Metrics & Logs.
- RDS/Aurora → CloudWatch for performance insights.
- CloudWatch Alarm → SNS → Ops team notification.
- EventBridge → Lambda for automated remediation.

### Sample Question

A company wants to automatically stop EC2 instances during off-hours to save cost. Which service combination is best?

- A) CloudTrail + Config
- B) CloudWatch + EventBridge + Lambda ✅
- C) S3 + CloudWatch Logs
- D) Inspector + CloudTrail

---

## AWS CloudTrail

### Key Features

- Captures all **API calls** (management & data events).
- Stores logs in S3 with optional CloudWatch Logs integration.
- Supports **multi-region trails** and **organization trails**.
- Provides audit history for compliance.

### Exam Architectures

- CloudTrail → S3 (long-term audit).
- CloudTrail + CloudWatch Logs → Real-time monitoring of IAM activity.
- CloudTrail → S3 + Athena → Query API activity.

### Sample Question

Your security team must detect when IAM users create access keys. What should you use?

- A) Config Rules
- B) CloudTrail with CloudWatch Logs + Metric Filter ✅
- C) CloudWatch Dashboards
- D) Inspector

---

## AWS Config

### Key Features

- Tracks resource configuration changes.
- Evaluates compliance using **Config Rules** (managed or custom via Lambda).
- Provides snapshots of current resource states.
- Integrates with CloudTrail for historical view.

### Exam Architectures

- Config → Detect non-encrypted S3 bucket → Remediation via Lambda.
- Config + Security Hub → Central compliance dashboard.
- Config Aggregator → Multi-account, multi-region compliance.

### Sample Question

Your company requires all EBS volumes to be encrypted. Which solution ensures ongoing compliance?

- A) CloudTrail Event Logs
- B) AWS Config with managed rule `encrypted-volumes` ✅
- C) CloudWatch Alarm on EC2 metrics
- D) Trusted Advisor

---

## Architecture Diagram (Text Sketch)

```
Users → AWS API Calls → CloudTrail → S3 + CloudWatch Logs
EC2/RDS/Lambda → CloudWatch Metrics → CloudWatch Alarms → SNS/Lambda
AWS Resources → AWS Config → Compliance Dashboard → Security Hub
```

---

## Flashcards

**Q: What does CloudWatch monitor?**  
A: Metrics, Logs, Alarms, Events.

**Q: What does CloudTrail capture?**  
A: API calls and account activity.

**Q: What does AWS Config do?**  
A: Resource inventory, compliance evaluation, and change history.

**Q: Explicit Deny vs Allow in IAM?**  
A: Explicit Deny always overrides Allow.

**Q: Which service shows “who did what, when, and from where”?**  
A: AWS CloudTrail.

---

## Key Takeaways for the Exam

- Use **CloudWatch** for operational monitoring.
- Use **CloudTrail** for auditing & API visibility.
- Use **Config** for compliance and drift detection.
- Remember integration patterns (e.g., Config + Lambda remediation).
- Always think “which service gives me visibility, compliance, or automation?”.
