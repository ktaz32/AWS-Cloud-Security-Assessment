# AWS Cloud Security Assessment & Misconfiguration Hunt

A hands-on cloud security project focused on identifying, assessing, remediating, and validating security weaknesses within a controlled **Amazon Web Services (AWS)** environment.

The project simulates a professional cloud security assessment and demonstrates practical skills across **AWS security, Identity and Access Management (IAM), least-privilege design, security logging, misconfiguration analysis, risk assessment, remediation, and security validation**.

> **Environment:** Personal AWS lab containing only controlled test resources and synthetic data.

---

## Project Objectives

The objectives of this project are to:

* Build a controlled AWS environment for security testing
* Identify cloud security misconfigurations
* Assess IAM users, roles, policies, and permissions
* Detect excessive and unused privileges
* Apply least-privilege access principles
* Analyze AWS activity using CloudTrail
* Use IAM Access Analyzer to evaluate access and policy risks
* Perform automated cloud-security assessments
* Prioritize findings according to technical and business risk
* Remediate identified weaknesses
* Validate security improvements after remediation
* Produce technical and executive-level security documentation

---

## Skills Demonstrated

* AWS Cloud Security
* Identity and Access Management (IAM)
* Least-Privilege Access Control
* Cloud Security Assessments
* Cloud Misconfiguration Analysis
* AWS CloudTrail
* IAM Access Analyzer
* Security Logging and Monitoring
* Security Control Validation
* Risk Assessment
* Vulnerability Prioritization
* Remediation Planning
* Security Architecture
* Technical Documentation
* Executive Risk Communication

---

## Technologies

| Technology          | Purpose                                                   |
| ------------------- | --------------------------------------------------------- |
| AWS IAM             | Identity, roles, permissions, and access-control analysis |
| Amazon S3           | Cloud storage security assessment                         |
| Amazon EC2          | Compute and network-security testing                      |
| AWS CloudTrail      | API and account activity logging                          |
| IAM Access Analyzer | External access, policy, and privilege analysis           |
| AWS Security Groups | Network-access control assessment                         |
| Prowler             | Automated AWS security assessment                         |
| AWS CLI             | Cloud administration and evidence collection              |
| Git / GitHub        | Version control and project documentation                 |

---

## Assessment Methodology

The project follows a structured security-assessment workflow:

```text
Environment Build
       ↓
Asset Discovery
       ↓
Configuration Review
       ↓
Misconfiguration Identification
       ↓
Evidence Collection
       ↓
Risk Assessment
       ↓
Finding Prioritization
       ↓
Remediation
       ↓
Security Validation
       ↓
Final Reporting
```

Each security finding is documented with:

* Finding ID
* Description
* Affected resource
* Evidence
* Security impact
* Likelihood
* Severity
* Relevant security principle
* Recommended remediation
* Remediation actions
* Validation evidence
* Final status

---

## Lab Architecture

The initial AWS lab contains:

```text
                    AWS Account
                         │
             ┌───────────┴───────────┐
             │                       │
            IAM                  CloudTrail
             │                       │
      ┌──────┼──────┐                │
      │      │      │                ↓
 Developer Security Auditor      Activity Logs
   Role     Role    Role
      │
      ├──────────────┐
      │              │
      ↓              ↓
     S3             EC2
      │              │
      └──── Security Groups
```

The environment will intentionally contain controlled security weaknesses that are documented and remediated during the assessment.

---

## Planned Security Findings

| ID     | Finding                            | Area    | Initial Status |
| ------ | ---------------------------------- | ------- | -------------- |
| AWS-01 | Excessive IAM permissions          | IAM     | Planned        |
| AWS-02 | Wildcard resource permissions      | IAM     | Planned        |
| AWS-03 | Unused or unnecessary access       | IAM     | Planned        |
| AWS-04 | S3 access-control misconfiguration | Storage | Planned        |
| AWS-05 | Overly permissive security group   | Network | Planned        |
| AWS-06 | Security logging or monitoring gap | Logging | Planned        |

Additional findings may be added as the assessment develops.

---

## IAM Least-Privilege Assessment

A major component of this project is evaluating excessive AWS permissions and redesigning access according to the **principle of least privilege**.

Example:

### Initial Permission Model

```text
DeveloperRole
      │
      ↓
Broad S3 Permissions
      │
      ↓
Multiple AWS Resources
```

### Remediated Model

```text
DeveloperRole
      │
      ↓
Required S3 Actions Only
      │
      ↓
Specific Project Resources
```

The remediated design aims to reduce unnecessary privileges and minimize the potential impact of credential compromise.

---

## Findings

Detailed findings are maintained in the [`findings/`](findings/) directory.

Each finding contains the complete analyst reasoning process from discovery through remediation and validation.

Example:

```text
AWS-01
Excessive IAM Permissions Assigned to Developer Role

Severity: High

Status:
Identified → Assessed → Remediated → Validated
```

---

## Automated Security Assessment

The project also incorporates automated cloud-security assessment using **Prowler**.

Automated findings are not accepted blindly.

Results are manually reviewed to determine:

* Whether the finding is valid
* Actual exposure
* Potential impact
* Environmental context
* Remediation priority
* False-positive considerations

This ensures the project demonstrates **security analysis**, rather than simply running a scanning tool.

---

## Risk Assessment

Findings are evaluated using factors including:

* Technical severity
* Exposure
* Exploitability
* Access requirements
* Potential blast radius
* Data sensitivity
* Business impact
* Existing compensating controls

Example:

| Finding                  | Likelihood | Impact | Priority |
| ------------------------ | ---------- | ------ | -------- |
| Excessive IAM privilege  | High       | High   | Critical |
| Public resource exposure | High       | High   | Critical |
| Unused permissions       | Medium     | Medium | Moderate |
| Logging gap              | Medium     | High   | High     |

---

## Remediation & Validation

Identifying a weakness is only the first half of the assessment.

For every applicable finding, the project documents:

```text
Finding
   ↓
Root Cause
   ↓
Remediation
   ↓
Reassessment
   ↓
Validation
   ↓
Closed / Residual Risk
```

Post-remediation validation is used to confirm that corrective actions reduced the identified risk without unnecessarily disrupting required functionality.

---

## Reporting

The project includes two reporting levels.

### Technical Assessment Report

Designed for security and engineering teams, including:

* Technical evidence
* IAM policies
* Configuration analysis
* CloudTrail activity
* Security-tool findings
* Risk analysis
* Remediation procedures
* Validation results

### Executive Summary

Designed for management and business stakeholders, summarizing:

* Overall security posture
* Highest-priority findings
* Business impact
* Key remediation actions
* Residual risks
* Recommended next steps

---

## Repository Structure

```text
AWS-Cloud-Security-Assessment/
├── architecture/
├── assessment/
├── findings/
├── iam/
├── cloudtrail/
├── prowler/
├── remediation/
├── evidence/
├── reports/
└── docs/
```

---

## Project Status

**Status:** In Progress

### Current Phase

* [ ] Secure AWS lab account
* [ ] Build initial AWS environment
* [ ] Configure CloudTrail logging
* [ ] Configure IAM lab identities
* [ ] Complete AWS-01 IAM assessment
* [ ] Perform automated assessment
* [ ] Remediate findings
* [ ] Validate remediation
* [ ] Complete technical report
* [ ] Complete executive summary

---

## Security & Ethical Scope

All security testing in this repository is performed against:

* Personally controlled AWS resources
* Deliberately configured lab systems
* Synthetic or non-sensitive data

No testing is conducted against third-party systems without authorization.

Secrets, credentials, account identifiers, access keys, and sensitive cloud information are excluded from this repository.

---

## Disclaimer

This repository is an educational cybersecurity portfolio project. The AWS environment is intentionally configured with selected security weaknesses for controlled analysis and remediation.

The project is not affiliated with or endorsed by Amazon Web Services.
