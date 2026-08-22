# AWS Lab Architecture

## Overview

This document summarizes the AWS lab architecture used for the six completed security assessment cases in this repository.

The environment was intentionally designed to demonstrate common cloud-security weaknesses across IAM, Amazon S3, Amazon EC2, VPC security groups, and AWS CloudTrail.

All resources were created in a personally controlled AWS lab using synthetic, non-sensitive data.

---

## High-Level Architecture

```text
AWS Account
│
├── IAM
│   ├── DeveloperRole
│   ├── ReportReaderRole
│   ├── LegacyAppRole
│   ├── DataAnalystRole
│   └── AuditReaderRole
│
├── Amazon S3
│   ├── khaled-cloud-security-lab-01
│   ├── khaled-cloud-security-control-01
│   ├── khaled-cloud-security-reports-01
│   ├── khaled-cloud-security-legacy-app-01
│   ├── khaled-cloud-security-finance-01
│   └── khaled-cloud-security-audit-01
│
├── Amazon EC2
│   └── aws05-ssh-test
│       └── AWS05-OpenSSH-SG
│
└── AWS CloudTrail
    └── AWS06-S3-DataEvent-Trail
        └── S3 read data-event logging
```

---

## Security Assessment Mapping

| Case | Primary Resource | Security Control Tested |
|---|---|---|
| AWS-01 | `DeveloperRole` / S3 | Excessive managed IAM permissions |
| AWS-02 | `ReportReaderRole` / S3 | Wildcard resource scope |
| AWS-03 | `LegacyAppRole` | Stale/unused IAM permissions |
| AWS-04 | `DataAnalystRole` / Finance S3 bucket | Resource-based authorization |
| AWS-05 | `aws05-ssh-test` / Security Group | Network exposure |
| AWS-06 | `AuditReaderRole` / Audit S3 bucket / CloudTrail | Data-event logging and detection visibility |

---

## Authorization Paths

### Identity-Based IAM

```text
IAM Role
   |
   v
Identity Policy
   |
   v
AWS Resource
```

Used primarily in:

- AWS-01
- AWS-02
- AWS-03
- AWS-06

### Resource-Based Authorization

```text
IAM Role
   |
   v
S3 Bucket Policy
   |
   v
S3 Resource
```

Used in:

- AWS-04

The AWS-04 case demonstrates that a role can have effective access even when no S3 identity policy is directly attached.

---

## Network Security Path

AWS-05 evaluated the exposure created by an EC2 security group.

### Before

```text
Internet
0.0.0.0/0
    |
    v
TCP/22
    |
    v
AWS05-OpenSSH-SG
    |
    v
aws05-ssh-test
```

### After

```text
Trusted Administrator
x.x.x.x/32
    |
    v
TCP/22
    |
    v
AWS05-OpenSSH-SG
    |
    v
aws05-ssh-test

All other IPv4 sources
    |
    X
```

---

## Logging and Detection Path

AWS-06 evaluated whether S3 object-level access was observable.

### Before

```text
AuditReaderRole
      |
      v
S3 GetObject
      |
      X
Not visible in standard
CloudTrail management-event history
```

### After

```text
AuditReaderRole
      |
      v
S3 GetObject
      |
      v
CloudTrail S3 Data Event
      |
      v
Recorded audit evidence
```

The final CloudTrail record captured fields such as:

```text
eventSource: s3.amazonaws.com
eventName: GetObject
role: AuditReaderRole
bucketName: khaled-cloud-security-audit-01
key: audit-sensitive.txt
httpStatusCode: 200
```

---

## Design Principles

The lab was built around five security principles:

1. **Least Privilege** — permissions should be limited to required actions and resources.
2. **Effective Authorization** — access must be validated across both identity- and resource-based policy layers.
3. **Attack-Surface Reduction** — network exposure should be limited to explicitly trusted sources.
4. **Entitlement Hygiene** — unused permissions should be removed over time.
5. **Detection Visibility** — security-relevant activity must be logged at the required event layer.

---

## Scope Boundaries

This architecture does not represent a production enterprise AWS landing zone.

The lab intentionally excludes areas such as:

- AWS Organizations and SCPs
- AWS Security Hub
- Amazon GuardDuty
- AWS Config
- production KMS key-policy management
- enterprise SIEM integration
- multi-account network architecture

Those areas are potential future extensions and are not represented as completed work in this repository.
