# Executive Summary

## Assessment Overview

A security assessment was performed in a personally controlled Amazon Web Services (AWS) lab to evaluate identity and access management, S3 authorization, EC2 network exposure, entitlement hygiene, and security logging.

Six deliberately introduced weaknesses were assessed using a repeatable workflow of configuration review, effective-access testing, risk assessment, remediation, and post-remediation validation.

All six findings were successfully remediated and validated.

---

## Scope

The completed assessment covered:

- AWS Identity and Access Management (IAM)
- IAM roles and custom/managed policies
- Amazon S3 identity-based and resource-based authorization
- IAM service last-accessed information
- Amazon EC2
- VPC security groups
- AWS CloudTrail management events
- CloudTrail S3 data events

All resources and data were created specifically for the lab.

---

## Overall Result

**Initial posture:** Material weaknesses intentionally present for controlled testing  
**Final posture:** All documented findings remediated and validated

| Severity | Findings | Remediated |
|---|---:|---:|
| Critical | 0 | 0 |
| High | 4 | 4 |
| Medium | 2 | 2 |
| Low | 0 | 0 |
| **Total** | **6** | **6** |

---

## Findings Summary

| ID | Finding | Severity | Outcome |
|---|---|---|---|
| AWS-01 | Excessive IAM permissions assigned to a developer role | High | Broad S3 access replaced with resource-scoped least privilege |
| AWS-02 | Wildcard resource scope in a custom IAM policy | High | Wildcard scope removed; required reports access retained |
| AWS-03 | Unused/stale DynamoDB permissions | Medium | Unused service permissions removed; required S3 access retained |
| AWS-04 | Unintended finance-bucket access through a resource-based policy | High | Bucket-policy authorization removed and access denied |
| AWS-05 | SSH open to the IPv4 internet | High | Source restricted from `0.0.0.0/0` to a trusted `/32` |
| AWS-06 | Missing S3 object-level audit visibility | Medium | S3 read data events enabled and `GetObject` telemetry validated |

---

## Key Risk Themes

### 1. Excessive Authorization

The first four cases showed that excessive access can arise through several distinct mechanisms:

- broad AWS-managed permissions;
- wildcard resource scope;
- stale service entitlements;
- resource-based S3 policies.

The assessment therefore reinforced the need to evaluate **effective authorization**, not merely policies attached directly to an identity.

### 2. Network Attack Surface

AWS-05 demonstrated unnecessary administrative exposure through a security group allowing SSH from `0.0.0.0/0`.

Restricting the source to a trusted `/32` reduced exposure while preserving the required management path.

### 3. Detection and Audit Coverage

AWS-06 demonstrated that standard CloudTrail management-event visibility does not provide all object-level S3 activity.

Enabling S3 data events closed the audit gap and produced a successful `GetObject` record containing the role, bucket, object key, and request outcome.

---

## Remediation Outcomes

Corrective actions included:

- replacing broad managed permissions with custom least-privilege policies;
- restricting IAM resources to explicit ARNs;
- removing stale permissions based on observed service use;
- removing unintended S3 resource-policy principals;
- narrowing SSH source CIDRs;
- enabling required S3 data-event telemetry.

Every remediation was followed by a validation test.

```text
Finding
   ↓
Root Cause
   ↓
Scoped Remediation
   ↓
Repeat Original Test
   ↓
Validate Required Function
   ↓
Close Finding
```

---

## Security Posture Improvement

The project produced measurable improvements across four control domains:

| Control Domain | Initial Weakness | Validated Improvement |
|---|---|---|
| Identity | Excessive/wildcard/stale permissions | Scoped and reduced permissions |
| Resource authorization | Unintended S3 bucket-policy access | Unauthorized path removed |
| Network | Internet-wide SSH exposure | Single-host trusted source |
| Monitoring | Missing S3 object-level visibility | `GetObject` data event captured |

---

## Residual Risk

No residual risk was intentionally accepted for the six documented lab findings.

The lab does not claim full AWS security coverage. Areas outside the completed scope include workload vulnerability management, KMS key-policy review, GuardDuty, AWS Config, Security Hub, organization-level SCP analysis, and production-grade SIEM integration.

---

## Conclusion

The assessment demonstrates that cloud security depends on understanding the **effective state** of an environment across identity policies, resource policies, network controls, and telemetry.

The strongest outcome of the project is not simply that six weaknesses were found, but that each weakness was:

1. reproduced or validated;
2. tied to a clear authorization, exposure, or visibility path;
3. remediated with a scoped control;
4. retested to verify the intended security outcome.

This evidence-driven workflow is directly applicable to cloud-security assessment, SOC investigations, and security engineering.
