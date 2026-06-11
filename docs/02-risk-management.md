# Document 2: Risk Management Summary

**Systems:** NP-001 Credit Scoring Engine · NP-002 Fraud Detection System
**EU AI Act Reference:** Article 9 (Risk Management System)
**Version:** 1.0 - July 2026
**Prepared by:** AI Governance Programme Office

---

## 1. Overview

Article 9 requires that high-risk AI systems be subject to a documented risk management system that is a continuous, iterative process throughout the system's entire lifecycle. This document records the risk management approach for both HIGH RISK systems in NorthPoint's AI portfolio.

This document should be read alongside:
- Phase 2 AI Risk Assessment - [franciscovfonseca/AI-Risk-Assessment](https://github.com/franciscovfonseca/AI-Risk-Assessment) - which provides the original likelihood/impact analysis for NP-001 and NP-002
- Phase 4 AI Incident Response - [franciscovfonseca/AI-Incident-Response](https://github.com/franciscovfonseca/AI-Incident-Response) - which documents the realised bias incident in NP-001 and the corrective action plan

---

## 2. NP-001 - Credit Scoring Engine Risk Register

### R1 - Discriminatory Credit Scoring Outcomes

**Description:** The model produces scores that systematically disadvantage applicants from protected groups due to proxy discrimination, training data bias or feature encoding.

**Status:** This risk was realised in Phase 4 (NP-INC-2026-001). The postcode feature was identified as an ethnicity proxy, removed and the model retrained. Enhanced controls are now in place for v2.0.

| | |
|---|---|
| **Likelihood** | Low (reduced from High following Phase 4 remediation) |
| **Impact** | Critical |
| **Risk level** | Medium |

**Controls in place (v2.0):**
- Postcode feature removed; annual proxy analysis of all remaining geographic and socioeconomic features
- Independent fairness audit completed before v2.0 deployment
- Ongoing demographic parity monitoring (quarterly)
- Underwriter override mechanism and customer challenge right (see Document 4)
- Corrective action plan from Phase 4 embedded in governance programme

**Residual risk:** Medium - bias risk is reduced but not fully eliminated by one remediation. Ongoing monitoring is the primary control.

---

### R2 - Over-reliance by Loan Underwriters

**Description:** Underwriters defer to NP-001 output without exercising genuine independent judgment, making the model's recommendation the de facto decision.

| | |
|---|---|
| **Likelihood** | Medium |
| **Impact** | High |
| **Risk level** | High |

**Controls:**
- Mandatory oversight training before system access is granted
- Feature contribution summary requires active review before a decision can be recorded
- Override rate monitoring - low override rates trigger coaching review
- Underwriter cannot submit a decision until the review step is completed in the system

**Residual risk:** Low

---

### R3 - Model Drift

**Description:** Predictive accuracy degrades as the applicant population, macroeconomic conditions or NorthPoint's lending criteria change over time.

| | |
|---|---|
| **Likelihood** | Medium (increases with time since revalidation) |
| **Impact** | High |
| **Risk level** | Medium |

**Controls:**
- Annual model revalidation cycle
- Performance metrics monitored quarterly (see Document 6)
- Off-cycle revalidation triggered by macroeconomic indicators

**Residual risk:** Low

---

### R4 - Explainability Failure

**Description:** Applicants who receive a decline decision cannot understand the basis for the decision or exercise their right to human review, creating legal exposure under UK GDPR Article 22 and FCA Consumer Duty.

| | |
|---|---|
| **Likelihood** | Low (controls designed in from v2.0 deployment) |
| **Impact** | Medium-High |
| **Risk level** | Medium |

**Controls:**
- Feature contribution summary provided for every decision
- Customer-facing decline letter includes plain-language basis for decision
- GDPR Article 22 notice included in all decline communications with instructions for requesting human review

**Residual risk:** Low

---

## 3. NP-002 - Fraud Detection System Risk Register

### R1 - High False Positive Rate Causing Customer Harm

**Description:** Legitimate transactions are incorrectly held, preventing customers from accessing their funds and causing financial harm and loss of trust.

| | |
|---|---|
| **Likelihood** | Medium (inherent in probabilistic fraud detection) |
| **Impact** | High |
| **Risk level** | High |

**Controls:**
- False positive SLA: < 0.5% of flagged transactions at the current threshold
- 4-hour resolution target for all holds
- Fraud analyst review of every flagged transaction
- Customer dispute and escalation pathway
- Monthly false positive rate reported to the Board

**Residual risk:** Low

---

### R2 - Demographic Disparity in Hold Rates

**Description:** The model flags transactions from certain customer demographic groups at higher rates, causing disproportionate disruption to specific customer segments - the same category of risk that materialised in NP-001.

**Status:** This risk was flagged in Phase 4. A parallel bias review of NP-002 was initiated in March 2026 and is ongoing. Target completion: Q4 2026.

| | |
|---|---|
| **Likelihood** | Medium (postcode is used as a geographic feature - same risk category as the NP-001 feature that caused the Phase 4 incident) |
| **Impact** | High |
| **Risk level** | High |

**Controls:**
- Bias review in progress
- Enhanced monitoring of hold rates by demographic proxy implemented pending review completion
- If review identifies a disparity above threshold, containment and remediation will follow the Phase 4 incident response model

**Residual risk:** High - pending completion of bias review. This is the primary open risk in the NorthPoint AI portfolio.

---

### R3 - Novel Fraud Pattern Evasion

**Description:** The model fails to detect genuinely novel fraud techniques until a retraining cycle captures them, leaving a detection gap.

| | |
|---|---|
| **Likelihood** | High (fraud patterns evolve continuously) |
| **Impact** | Medium |
| **Risk level** | Medium |

**Controls:**
- Monthly fraud intelligence review by the Financial Crime team
- Retraining triggered by emerging pattern detection
- Fraud analyst investigation supplements model output for complex and unusual cases

**Residual risk:** Medium - inherent in the nature of adversarial fraud; managed through continuous monitoring.

---

### R4 - System Unavailability During Peak Periods

**Description:** Model or infrastructure failure during high-volume transaction periods creates fraud detection gaps.

| | |
|---|---|
| **Likelihood** | Low |
| **Impact** | High |
| **Risk level** | Medium |

**Controls:**
- 99.9% uptime SLA with infrastructure team
- Rule-based fallback fraud detection active when ML model is unavailable
- Incident response procedure for system outage

**Residual risk:** Low

---

## 4. Pre-Deployment Evaluation Status

### NP-001 v2.0 - Redeployment (June 2026)

| Evaluation | Owner | Status |
|---|---|---|
| Independent fairness audit (post-remediation) | External evaluator | Complete ✓ |
| Proxy analysis of all remaining features | ML Engineering and AGPO | Complete ✓ |
| Performance validation vs. v1.0 baseline | ML Engineering | Complete ✓ |
| Legal review - UK Equality Act and FCA Consumer Duty | Legal | Complete ✓ |
| AGPO sign-off | AGPO | Complete ✓ |
| AI Governance Committee approval for redeployment | AGC | Complete ✓ |

### NP-002 v1.4 - Current Deployment

| Evaluation | Owner | Status |
|---|---|---|
| Performance validation | ML Engineering | Complete ✓ |
| False positive SLA confirmation | Head of Financial Crime | Complete ✓ |
| Annex IV documentation completion | AGPO | Complete - this document ✓ |
| Bias review (geographic features) | AGPO and ML Engineering | In progress - Q4 2026 |

---

## 5. Residual Risk Summary

| System | Overall Residual Risk | Primary Basis |
|---|---|---|
| NP-001 v2.0 | Medium | Bias risk reduced but requires ongoing monitoring; explainability scope limitations acknowledged |
| NP-002 v1.4 | Medium-High | False positive controls in place; demographic disparity review pending |

Both residual risk positions have been reviewed and accepted by the AI Governance Committee as consistent with NorthPoint's risk appetite and Article 9(4) requirements. The NP-002 residual risk position will be re-evaluated following completion of the bias review.

---

*Risk management is a continuous process. This document is updated following any material incident, material system change or annual review.*
