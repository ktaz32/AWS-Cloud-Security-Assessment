# AWS-XX — Finding Title

## Finding Information

| Field           | Value                             |
| --------------- | --------------------------------- |
| Finding ID      | AWS-XX                            |
| Category        | IAM / Storage / Network / Logging |
| Severity        | Critical / High / Medium / Low    |
| Status          | Open / Remediated / Accepted      |
| AWS Service     | Service Name                      |
| Date Identified | YYYY-MM-DD                        |

---

## Executive Summary

Briefly explain the security weakness, why it matters, and the potential impact if exploited.

Keep this section understandable to a non-technical stakeholder.

---

## Observation

Describe exactly what was identified.

Include:

* Affected AWS resource
* Relevant configuration
* Permissions or settings
* How the issue was discovered

---

## Evidence

Document the evidence supporting the finding.

Possible evidence includes:

* AWS Console screenshots
* AWS CLI output
* IAM policy JSON
* CloudTrail events
* IAM Access Analyzer output
* Prowler findings

> Sensitive identifiers, credentials, account numbers, and access keys must be sanitized before publication.

---

## Technical Analysis

Explain why the configuration creates security risk.

Address:

* What access is currently allowed?
* What access is actually required?
* What could an attacker do if the identity or resource were compromised?
* What is the potential blast radius?
* Are compensating controls present?

---

## Risk Assessment

### Likelihood

**Rating:** Low / Medium / High

Explain why.

### Impact

**Rating:** Low / Medium / High / Critical

Explain potential consequences.

### Overall Severity

**Severity:** Critical / High / Medium / Low

---

## Security Principle

Relevant principles may include:

* Least Privilege
* Defense in Depth
* Separation of Duties
* Secure by Default
* Zero Trust
* Logging and Monitoring
* Minimize Attack Surface

---

## Recommendation

Describe the recommended remediation.

The recommendation should be:

* Specific
* Actionable
* Proportionate to risk
* Technically realistic

---

## Remediation

Document the remediation that was performed.

### Before

Describe the vulnerable configuration.

### After

Describe the corrected configuration.

Include sanitized configuration examples where useful.

---

## Validation

Explain how the remediation was tested.

Examples:

* IAM policy re-evaluation
* Access Analyzer reassessment
* AWS CLI permission testing
* Prowler rescanning
* CloudTrail validation
* Manual configuration review

---

## Result

**Final Status:** Remediated / Accepted Risk / Open

Summarize whether the vulnerability was successfully resolved.

---

## Lessons Learned

Document what this finding demonstrated about:

* AWS security
* IAM
* Misconfiguration risk
* Security assessment methodology
* Detection or monitoring
* Risk prioritization

---

## References

Add official AWS documentation and other authoritative references used during the assessment.
