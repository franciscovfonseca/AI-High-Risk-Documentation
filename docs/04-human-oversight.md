# Document 4: Human Oversight Mechanisms

**Systems:** NP-001 Credit Scoring Engine · NP-002 Fraud Detection System
**EU AI Act Reference:** Article 14 (Human Oversight)
**Version:** 1.0 — July 2026
**Prepared by:** AI Governance Programme Office

---

## 1. Purpose

Article 14 requires that high-risk AI systems be designed to enable effective human oversight — allowing humans to understand the system's capabilities and limitations, monitor its operation, detect failures or harmful outputs, intervene and override and decide not to use the AI output in a specific case.

This document describes how NorthPoint implements these requirements for both NP-001 and NP-002.

---

## 2. Oversight Principles

NorthPoint's human oversight model for both systems is built on three principles consistent with the governance framework established in Phase 3:

**1. The AI informs; the human decides.**
No credit decision or permanent account action may result from AI output alone. A qualified human must review the AI output and make an active, documented decision.

**2. Override is expected, not exceptional.**
Overriding an AI recommendation is the normal exercise of professional judgment. Systems, training and culture must make override feel routine and supported — not a challenge to the model's authority.

**3. Oversight must be auditable.**
Every review, every override and every decision must be logged. Oversight that cannot be demonstrated has not occurred.

---

## 3. NP-001 — Loan Underwriter Oversight

### 3.1 Oversight Roles

| Role | Oversight Responsibility |
|---|---|
| **Loan Underwriter** (Primary) | Reviews AI score, risk tier and feature contribution summary before any decision. Makes the credit decision. Documents reasoning for any divergence from the AI recommendation. |
| **Senior Underwriter** (Applications ≥ £25,000) | Reviews the underwriter's recommendation and AI output for higher-value applications. May require additional evidence before approving. |
| **Head of Credit Risk** (Governance) | Monthly override rate review. Quarterly fairness monitoring review. Escalation point for underwriter concerns about system behaviour. Annual oversight effectiveness attestation. |

### 3.2 Oversight Workflow

```
[1] Applicant submits loan application
     ↓
[2] NP-001 v2.0 generates credit score, risk tier and feature contribution summary
     ↓
[3] UNDERWRITER REVIEW
     ├── Reviews score, tier and all feature contributions
     ├── Considers context not captured in the model (applicant circumstances, market factors)
     ├── Makes decision: approve / decline / refer for additional information
     └── Documents decision and any divergence from AI recommendation
     ↓
[4] For applications ≥ £25,000: SENIOR UNDERWRITER REVIEW
     ├── Reviews underwriter recommendation and AI output
     └── May modify or approve the recommendation
     ↓
[5] DECISION COMMUNICATED TO APPLICANT
     ├── Approval: terms confirmed
     └── Decline: reason summary + GDPR Article 22 notice + right to request human review
     ↓
[6] OUTCOME LOGGED
     └── AI recommendation, underwriter decision and any divergence retained for 7 years
```

### 3.3 Override and Customer Challenge

**Internal override:** Underwriters may override the AI recommendation for any reason. Override requires a documented reason in the override log (minimum 20 words).

**Customer challenge:** Declined applicants may request human review of their decision within 30 days of receiving the decline communication. The request is reviewed by a senior underwriter who did not make the original decision. A written response is provided within 10 business days.

### 3.4 System Features Supporting Oversight

| Feature | Purpose |
|---|---|
| Feature contribution summary | Enables underwriters to understand why the AI produced the score it did — and to identify where they disagree |
| Full application display alongside AI output | Underwriters see the complete application, not just the AI's summary |
| Active confirmation required | System requires underwriters to click "Confirm decision" — no passive acceptance of AI output |
| Override logging | All divergences from AI recommendations are automatically logged with timestamp and reviewer ID |
| Low-confidence flag | Where the model's confidence is low for a specific application, this is flagged to the underwriter |

### 3.5 Oversight Effectiveness Metrics

| Metric | Target | Alert Threshold |
|---|---|---|
| Override rate | 10-40% of applications | < 5% triggers coaching review (potential nominal oversight) |
| Challenge rate | < 1% of declined applications | > 2% triggers fairness investigation |
| Training currency | 100% of underwriters current | < 100% triggers access suspension for lapsed users |

---

## 4. NP-002 — Fraud Analyst Oversight

### 4.1 Oversight Roles

| Role | Oversight Responsibility |
|---|---|
| **Fraud Analyst** (Primary) | Reviews all flagged transactions. Confirms or clears the hold within 4 business hours. Documents the decision and reasoning. |
| **Senior Fraud Analyst** | Reviews complex cases and disputed holds. Escalation point for borderline decisions requiring additional investigation. |
| **Head of Financial Crime** (Governance) | Monthly false positive rate review. Oversight of demographic hold rate monitoring. Annual oversight effectiveness attestation. |

### 4.2 Oversight Workflow

```
[1] Transaction submitted for processing
     ↓
[2] NP-002 v1.4 generates fraud score and pass/flag recommendation
     ↓
[3a] Score < 85: TRANSACTION PASSES
     └── Logged; no hold applied; transaction processed normally
     ↓
[3b] Score ≥ 85: AUTOMATIC TEMPORARY HOLD APPLIED
     └── Customer notified of hold via app and SMS
     ↓
[4] FRAUD ANALYST REVIEW (within 4 business hours)
     ├── Reviews transaction details, fraud score and reason code
     ├── Considers customer history and broader account context
     ├── Decision: release hold / confirm hold and investigate / escalate to senior analyst
     └── Documents decision and reasoning in case management system
     ↓
[5] HOLD RESOLUTION
     ├── Released: customer notified; transaction processed
     └── Confirmed: investigation initiated; customer contacted for verification
     ↓
[6] OUTCOME LOGGED
     └── Score, hold decision, analyst outcome and resolution time retained for 7 years
```

### 4.3 Customer Challenge Mechanism

Customers who believe a hold was incorrectly applied may contact NorthPoint's Financial Crime team directly. Under FCA Consumer Duty, disputed holds must be resolved within 3 business days. Where a hold is confirmed to have been a false positive, the customer receives:

- Written acknowledgment and apology
- Compensation for any demonstrable financial harm caused by the hold (assessed case by case)
- A record of the correction in their account history

### 4.4 System Features Supporting Oversight

| Feature | Purpose |
|---|---|
| Reason code per flagged transaction | Directs analyst attention to the specific fraud signal(s) the model identified |
| Full transaction context display | Analysts see the complete transaction history alongside the AI flag — not just the flagged transaction |
| Hold timer | Visual indicator showing time elapsed since hold was applied — supports SLA compliance |
| Override logging | All analyst decisions (confirm/clear) automatically logged with timestamp and analyst ID |
| Demographic hold rate dashboard | Available to Head of Financial Crime for quarterly demographic parity review |

### 4.5 Oversight Effectiveness Metrics

| Metric | Target | Alert Threshold |
|---|---|---|
| Hold resolution time | ≤ 4 business hours | > 6 hours average triggers capacity review |
| False positive rate | < 0.5% of flagged transactions | > 0.5% triggers threshold recalibration review |
| Customer dispute rate | < 0.5% of holds | > 1% triggers model and process review |
| Training currency | 100% of fraud analysts current | < 100% triggers access suspension for lapsed users |

---

## 5. Training Requirements

| System | Training Programme | Duration | Mandatory Before |
|---|---|---|---|
| NP-001 | Credit Risk AI Oversight Training | 3 hours | System access granted |
| NP-001 | Annual refresher | 1 hour | Annual recertification |
| NP-002 | Fraud Detection AI Oversight Training | 2 hours | System access granted |
| NP-002 | Annual refresher | 1 hour | Annual recertification |

Training covers: how the AI system works and what it does not do; the user's role as an overseer and what meaningful oversight looks like; when and how to override; how to handle customer challenges; and escalation paths.

Training completion is tracked in NorthPoint's LMS. Active system access is suspended for any user whose training certification has lapsed. This is a hard technical control — lapsed users cannot access either system until recertification is complete.

---

## 6. Governance Oversight

The Head of Credit Risk (NP-001) and Head of Financial Crime (NP-002) are responsible for oversight quality at the divisional level. The AI Governance Programme Office reviews aggregate oversight metrics quarterly and reports to the AI Governance Committee. Systemic oversight quality failures — low override rates, low review time, high challenge rates — are treated as potential AI incidents and escalated accordingly.

---

*This oversight procedure is a live document. Material changes to the workflow require AGPO review and AI Governance Committee approval.*
