# Assessment Methodology

## Purpose

This document defines the repeatable methodology used across the six AWS security findings in this repository.

The objective was not simply to identify insecure-looking configurations, but to establish whether they produced meaningful security impact, apply a scoped remediation, and then verify the resulting state.

---

## Core Workflow

```text
Identify
   ↓
Validate Effective Access or Exposure
   ↓
Assess Risk
   ↓
Determine Root Cause
   ↓
Remediate
   ↓
Repeat Original Test
   ↓
Validate Required Functionality
   ↓
Document Evidence
```

---

## 1. Define Intended State

Before testing, the expected access or security state is documented.

Examples:

- a developer role should access only one project bucket;
- a report reader should not access unrelated buckets;
- a legacy role should not retain unused service permissions;
- a data analyst should not read finance data;
- SSH should not be exposed to the entire internet;
- sensitive S3 reads should generate usable audit telemetry.

This prevents the assessment from treating every technically possible action as a security finding.

---

## 2. Inspect the Configuration

Relevant AWS configuration is reviewed to identify the control path involved.

Depending on the case, this includes:

- IAM managed policies;
- custom IAM policies;
- resource ARNs;
- IAM role service last-accessed information;
- S3 bucket policies;
- EC2 security groups;
- CloudTrail event configuration.

The goal is to understand **why** access or exposure exists.

---

## 3. Validate Effective State

Configuration review alone is not treated as sufficient evidence.

Where practical, the original security condition is tested directly.

Examples include:

```text
Attempt object access
Attempt bucket listing
Inspect role service activity
Review security-group source scope
Generate an S3 object-read event
```

The test confirms whether the suspected weakness affects the effective state of the environment.

---

## 4. Assess Risk

Each finding is evaluated using two primary dimensions:

### Likelihood

How plausible is abuse of the weakness?

Factors may include:

- public accessibility;
- breadth of permission;
- ease of role assumption;
- common automated scanning;
- frequency of use;
- whether the weakness is directly exploitable.

### Impact

What could happen if the weakness were abused?

Potential effects include:

- unauthorized data access;
- increased blast radius;
- administrative access;
- credential theft;
- lateral movement;
- loss of forensic visibility;
- violation of segregation-of-duty expectations.

The final severity reflects the combined technical context rather than the configuration label alone.

---

## 5. Determine Root Cause

The assessment distinguishes the visible symptom from the actual cause.

Examples:

| Finding | Root Cause |
|---|---|
| AWS-01 | Broad AWS-managed policy |
| AWS-02 | `Resource: "*"` in custom IAM policy |
| AWS-03 | Legacy unused service permissions |
| AWS-04 | S3 resource-based policy granting an unintended principal |
| AWS-05 | SSH source set to `0.0.0.0/0` |
| AWS-06 | S3 object-level data events not collected in the required logging path |

---

## 6. Apply a Scoped Remediation

Remediation is designed to remove the identified weakness while preserving required functionality.

Examples:

- replace broad S3 access with explicit bucket/object ARNs;
- remove wildcard resource scope;
- remove only unused service actions;
- remove the unintended bucket-policy principal;
- restrict SSH to a trusted `/32`;
- enable targeted S3 read data-event logging.

The remediation should be narrower than simply removing all access.

---

## 7. Repeat the Original Test

The same or equivalent validation test is repeated after remediation.

This is critical because:

```text
Policy changed
    does not automatically mean
Effective risk changed
```

A remediation is considered successful only when the original unwanted access or exposure is no longer present.

---

## 8. Validate Required Functionality

Where a legitimate function must remain, it is explicitly retested.

Examples:

- intended S3 bucket remains readable;
- required upload still succeeds;
- report bucket remains accessible;
- legacy application S3 access still works;
- SSH remains available to the trusted administrative source;
- object reads remain possible while audit telemetry is captured.

This avoids solving a security problem by unnecessarily breaking the workload.

---

## 9. Capture Evidence

Each finding includes before-and-after evidence.

Typical evidence includes:

```text
Before:
- insecure policy
- effective access
- broad exposure
- missing telemetry

After:
- remediated policy/configuration
- denied unintended access
- retained intended access
- confirmed security telemetry
```

Evidence filenames are numbered to preserve the investigation sequence.

---

## 10. Documentation Standard

Each finding report aims to include:

- finding metadata;
- executive summary;
- intended state;
- observed condition;
- technical analysis;
- risk assessment;
- root cause;
- remediation;
- post-remediation validation;
- before/after comparison;
- evidence index;
- lessons learned;
- ethical scope.

This structure is intended to resemble the reasoning expected in cloud-security assessment and SOC documentation.

---

## Evidence Quality Principles

Evidence should be:

- directly relevant to the claim;
- sufficient to reproduce the reasoning;
- sanitized before public publication;
- ordered chronologically where possible;
- accompanied by explanation rather than posted without context.

Screenshots are treated as supporting artifacts, not substitutes for analysis.

---

## Sanitization Standard

Before publication, screenshots and raw logs should be reviewed for unnecessary environment-specific values.

Typical redactions include:

- AWS account IDs;
- public IP addresses;
- private IP addresses;
- access-key identifiers;
- instance IDs;
- VPC IDs;
- security-group IDs;
- security-group rule IDs;
- public/private DNS names;
- full account-specific ARNs where unnecessary.

Useful security evidence should remain visible, for example:

```text
0.0.0.0/0
/32
SSH
TCP/22
GetObject
AuditReaderRole
bucket and object names used in the lab
```

---

## Ethical Scope

All tests are performed only against personally controlled AWS resources intentionally created for the lab.

The methodology excludes:

- unauthorized scanning;
- third-party resource testing;
- production customer data;
- credential sharing;
- destructive testing outside the lab.

---

## Completion Criteria

A finding is closed only when all applicable conditions are met:

- [x] Security weakness identified
- [x] Effective state validated
- [x] Risk explained
- [x] Root cause identified
- [x] Remediation implemented
- [x] Original test repeated
- [x] Required functionality preserved
- [x] Evidence documented

The completed AWS-01 through AWS-06 cases satisfy this workflow.
