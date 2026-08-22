# AWS Cloud Security Assessment & Misconfiguration Hunt

A hands-on AWS security portfolio project documenting the identification, validation, remediation, and retesting of cloud-security weaknesses in a controlled lab environment.

Rather than presenting configuration screenshots alone, each case follows a repeatable assessment workflow:

```text
Identify → Validate Effective Access/Exposure → Assess Risk
        → Remediate → Retest → Document Evidence
```

> **Environment:** Personally controlled AWS lab using synthetic/non-sensitive data only.

---

## Project Summary

This project contains **six completed AWS security findings** spanning identity, resource-based authorization, network exposure, stale entitlements, and security telemetry.

| ID | Finding | Domain | Severity | Status |
|---|---|---|---|---|
| [AWS-01](findings/AWS-01-excessive-iam-permissions/) | Excessive IAM permissions assigned to `DeveloperRole` | IAM / S3 | High | ✅ Remediated & Validated |
| [AWS-02](findings/AWS-02-Wildcard%20Resource%20Permissions%20in%20a%20Custom%20IAM%20Policy/) | Wildcard resource permissions in a custom IAM policy | IAM / S3 | High | ✅ Remediated & Validated |
| [AWS-03](findings/AWS-03-Unused%20Stale%20IAM%20Permissions/) | Unused/stale DynamoDB permissions on a legacy role | IAM | Medium | ✅ Remediated & Validated |
| [AWS-04](findings/AWS-04-Unintended%20Access%20Through%20an%20S3%20Bucket%20Policy/) | Unintended S3 access through a resource-based policy | S3 / IAM | High | ✅ Remediated & Validated |
| [AWS-05](findings/AWS-05-Overly%20Permissive%20Security%20Group%20Exposing%20SSH/) | SSH exposed to `0.0.0.0/0` | EC2 / Network | High | ✅ Remediated & Validated |
| [AWS-06](findings/AWS-06-CloudTrail%20Logging%20Gap%20Detection%20Visibility%20Failure/) | Missing S3 object-level audit visibility | CloudTrail / S3 | Medium | ✅ Remediated & Validated |

**Completed findings:** 6  
**High severity:** 4  
**Medium severity:** 2  
**Validated remediations:** 6/6

---

## What This Project Demonstrates

### Identity & Access Management
- IAM role and policy analysis
- Effective-permission validation
- Least-privilege redesign
- Wildcard-resource reduction
- Stale entitlement removal
- Service last-accessed analysis
- Resource-based vs. identity-based authorization

### Cloud & Network Security
- S3 bucket-policy assessment
- EC2 security-group review
- Public management-plane exposure analysis
- CIDR-based source restriction
- Attack-surface reduction

### SOC / Detection Visibility
- CloudTrail management-event review
- S3 data-event configuration
- Controlled activity generation
- `GetObject` telemetry validation
- Audit-gap identification
- Investigation-oriented evidence collection

### Security Assessment Practice
- Before/after evidence collection
- Root-cause analysis
- Likelihood/impact assessment
- Remediation implementation
- Post-remediation testing
- Technical and executive reporting

---

## Assessment Methodology

Every case uses the same evidence-driven process:

```text
1. Define intended access/security state
                ↓
2. Inspect current configuration
                ↓
3. Test effective access or exposure
                ↓
4. Document the finding and risk
                ↓
5. Implement a scoped remediation
                ↓
6. Repeat the original test
                ↓
7. Validate required functionality
                ↓
8. Record final status and lessons learned
```

This is important because a policy that *looks* restrictive is not necessarily restrictive in practice, and a remediation is not complete until its effect is tested.

---

## Findings at a Glance

### AWS-01 — Excessive IAM Permissions

`DeveloperRole` originally received AWS-managed `AmazonS3FullAccess`, allowing access beyond its intended project bucket.

**Remediation:** Replaced broad managed permissions with a resource-scoped custom policy and validated that intended S3 operations continued while control-bucket access was denied.

**Key concept:** Least privilege and blast-radius reduction.

---

### AWS-02 — Wildcard Resource Permissions

`ReportReaderRole` used a custom policy whose S3 permissions were scoped to `"Resource": "*"`, allowing reads from unrelated buckets.

**Remediation:** Replaced wildcard scope with explicit bucket and object ARNs. Required reports access remained functional while unrelated buckets became inaccessible.

**Key concept:** Action scope is only half of least privilege; resource scope matters equally.

---

### AWS-03 — Unused / Stale IAM Permissions

`LegacyAppRole` contained required S3 permissions plus DynamoDB permissions that were not used during the tracking period.

**Remediation:** Removed the unnecessary DynamoDB actions while preserving required S3 access and verified the service disappeared from the role's allowed-service footprint.

**Key concept:** Entitlement drift and periodic access review.

---

### AWS-04 — Resource-Based S3 Authorization

`DataAnalystRole` had no attached S3 identity policy, yet it could read finance data because the S3 bucket policy explicitly granted the role access.

**Remediation:** Removed the unintended resource-policy authorization and confirmed both bucket listing and object access were denied afterward.

**Key concept:** `No attached IAM policy` does **not** mean `no effective access`.

---

### AWS-05 — Internet-Exposed SSH

A publicly addressed EC2 instance used a security group permitting:

```text
SSH / TCP 22 / 0.0.0.0/0
```

**Remediation:** Restricted SSH to a trusted `/32` administrative source and confirmed the remediated security group remained attached to the instance.

**Key concept:** A required service does not need to be reachable from everywhere.

---

### AWS-06 — CloudTrail S3 Data-Event Visibility Gap

Controlled access to `audit-sensitive.txt` was not represented as a `GetObject` event in the standard CloudTrail management-event history.

**Remediation:** Enabled S3 read data-event logging for the audit workload, repeated the access, and confirmed CloudTrail captured a successful `GetObject` data event identifying the role, bucket, key, and request outcome.

**Key concept:** CloudTrail being enabled does not automatically mean all resource-level activity is collected.

---

## Security Concepts Covered

| Concept | Demonstrated In |
|---|---|
| Principle of least privilege | AWS-01, AWS-02, AWS-03 |
| Identity-based policies | AWS-01, AWS-02, AWS-03 |
| Resource-based policies | AWS-04 |
| Effective authorization | AWS-01, AWS-02, AWS-04 |
| Entitlement drift | AWS-03 |
| IAM service last-accessed data | AWS-03 |
| Network least privilege | AWS-05 |
| Security groups / CIDR scope | AWS-05 |
| CloudTrail management events | AWS-06 |
| CloudTrail S3 data events | AWS-06 |
| Detection visibility | AWS-06 |
| Remediation validation | AWS-01 through AWS-06 |

---

## Repository Structure

```text
AWS-Cloud-Security-Assessment/
├── README.md
├── .gitignore
├── findings/
│   ├── README.md
│   ├── AWS-01-excessive-iam-permissions/
│   ├── AWS-02-Wildcard Resource Permissions in a Custom IAM Policy/
│   ├── AWS-03-Unused Stale IAM Permissions/
│   ├── AWS-04-Unintended Access Through an S3 Bucket Policy/
│   ├── AWS-05-Overly Permissive Security Group Exposing SSH/
│   └── AWS-06-CloudTrail Logging Gap Detection Visibility Failure/
├── iam/
│   ├── insecure-policies/
│   └── remediated-policies/
└── reports/
    └── executive-summary.md
```

Each finding contains its own `README.md`, evidence set, and applicable policy/configuration artifacts.

---

## IAM Policy Library

Reusable before/after policy artifacts are also maintained under [`iam/`](iam/):

```text
iam/
├── insecure-policies/
│   ├── FinanceBucketPolicy-Before.json
│   ├── LegacyAppMixedAccessPolicy.json
│   └── ReportReaderWildcardPolicy.json
└── remediated-policies/
    ├── AuditReaderS3Policy.json
    ├── DeveloperS3LeastPrivilege.json
    ├── LegacyAppS3OnlyPolicy.json
    └── ReportReaderLeastPrivilegePolicy.json
```

These policy files make the access-control changes reviewable independently of screenshots.

---

## Assessment Results

All six deliberately introduced findings were remediated and retested.

| Severity | Findings | Remediated |
|---|---:|---:|
| Critical | 0 | 0 |
| High | 4 | 4 |
| Medium | 2 | 2 |
| Low | 0 | 0 |
| **Total** | **6** | **6** |

The project should not be interpreted as a benchmark of a production AWS environment. The weaknesses were intentionally created in an isolated lab to demonstrate assessment methodology and remediation skills.

---

## Executive Reporting

A management-oriented summary is available at:

**[Executive Summary](reports/executive-summary.md)**

Individual case reports contain the technical evidence and analyst reasoning.

---

## Portfolio Takeaways

The six cases collectively demonstrate that cloud-security assessment requires more than checking whether a policy exists.

The key questions are:

```text
Who can access the resource?
Through which authorization path?
From which network source?
Is the permission still required?
Is the activity observable?
Did remediation actually change the effective state?
```

The project intentionally validates those questions through controlled testing rather than relying solely on configuration inspection.

---

## Security & Ethical Scope

All testing was performed against personally controlled AWS resources created for this lab.

- Only synthetic/non-sensitive test data was used.
- No third-party or production systems were tested.
- No unauthorized scanning was performed.
- Credentials and private keys are excluded from the repository.
- Screenshots intended for public publication are sanitized to remove unnecessary account-specific metadata.
- AWS resources should be stopped or deleted after testing when no longer required to minimize cost and exposure.

---

## Future Extensions

The six-case assessment is complete. Possible future extensions are intentionally separated from the completed scope:

- AWS Config / Security Hub control validation
- Prowler-assisted posture assessment with manual triage
- GuardDuty detection validation
- CloudTrail-to-SIEM ingestion
- detection-as-code for AWS telemetry
- Terraform-based secure/insecure lab deployment

These are **not represented as completed work in this repository** unless corresponding evidence is later added.

---

## Disclaimer

This repository is an educational cybersecurity portfolio project. The AWS environment intentionally contained selected weaknesses for controlled analysis and remediation.

The project is not affiliated with or endorsed by Amazon Web Services.
