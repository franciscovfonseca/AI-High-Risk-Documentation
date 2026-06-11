# Document 6: Performance Monitoring Plan

**Systems:** NP-001 Credit Scoring Engine · NP-002 Fraud Detection System  
**EU AI Act Reference:** Article 15 (Accuracy, Robustness, Cybersecurity) · Article 17 (Post-Market Monitoring)  
**Version:** 1.0 - July 2026  
**Prepared by:** AI Governance Programme Office  

---

## 1. Purpose

Article 17 requires deployers of high-risk AI systems to implement a post-market monitoring plan that systematically collects and analyses performance data throughout the system's operational lifetime. Article 15 requires that high-risk AI systems achieve an appropriate level of accuracy and robustness.

This document defines NorthPoint's monitoring plan for NP-001 and NP-002.

---

## 2. Monitoring Objectives

Post-deployment monitoring serves four objectives for both systems:

1. **Performance validation** - confirm each system continues to perform at or above accepted standards
2. **Fairness assurance** - detect emerging bias or disparate impact across customer groups before harm accumulates
3. **Oversight quality** - verify that human oversight is functioning as designed and not becoming nominal
4. **Drift detection** - identify when the customer population, market conditions or fraud patterns have shifted sufficiently that a system may need revalidation

> **Monitoring philosophy:** The Phase 4 incident demonstrated that a model can be monitored for accuracy for four years while producing discriminatory outcomes that are invisible to accuracy metrics alone. This monitoring plan is designed to prevent that failure from recurring. Fairness metrics are treated as first-class monitoring obligations - not optional additions to performance tracking.

---

## 3. NP-001 - Credit Scoring Engine Monitoring

### 3.1 Model Performance Metrics

| Metric | Definition | Target | Frequency | Alert Threshold |
|---|---|---|---|---|
| **Gini coefficient** | Standard credit discrimination measure (Gini = 2 × AUC - 1) | ≥ 0.45 | Quarterly | < 0.40 triggers revalidation review |
| **Default prediction accuracy** | % of defaults correctly classified at current risk tier thresholds | Baseline from v2.0 validation | Quarterly | > 10% decline from baseline |
| **Score distribution stability** | KS test comparing current score distribution to v2.0 launch distribution | p > 0.05 (no significant shift) | Monthly | p < 0.05 for two consecutive months |
| **Approval rate trend** | Overall approval rate across all applications | Baseline from v2.0 launch | Monthly | > 15% variation unexplained by application volume changes |

### 3.2 Fairness Metrics (Enhanced Post-Phase 4)

| Metric | Definition | Target | Frequency | Alert Threshold |
|---|---|---|---|---|
| **Demographic parity ratio** | Approval rate for protected group vs. reference group | ≥ 0.80 for all measurable groups | Quarterly | < 0.80 or declining trend over two consecutive quarters |
| **Score distribution by proxy group** | Mean score and standard deviation by gender proxy, age band and name-inferred ethnicity proxy | No statistically significant difference between groups | Quarterly | p < 0.05 in group mean difference |
| **Annual proxy analysis** | Cramér's V correlation between each model feature and protected characteristic proxies | No feature at Cramér's V > 0.30 | Annual | Any feature exceeding threshold triggers immediate review and possible removal |
| **Override rate by demographic group** | Whether underwriters override AI recommendations at systematically different rates across groups | No statistically significant pattern | Quarterly | Significant difference triggers fairness investigation |

### 3.3 NP-001 Revalidation Triggers

| Trigger | Action |
|---|---|
| Gini coefficient < 0.40 | Immediate revalidation review; possible suspension pending outcome |
| Demographic parity ratio < 0.80 for any measurable group | Escalation to AGPO; bias investigation initiated following Phase 4 incident response model |
| Annual proxy analysis identifies Cramér's V > 0.30 for any feature | Feature review; possible removal before next quarterly monitoring cycle |
| Macroeconomic shock - significant change in unemployment or credit conditions | Off-cycle revalidation |
| 24 months since last full revalidation | Scheduled revalidation |

---

## 4. NP-002 - Fraud Detection System Monitoring

### 4.1 Model Performance Metrics

| Metric | Definition | Target | Frequency | Alert Threshold |
|---|---|---|---|---|
| **False positive rate** | % of flagged transactions cleared by fraud analysts as legitimate | < 0.5% | Monthly | > 0.5% triggers threshold recalibration review |
| **Detection rate** | % of confirmed fraud cases that were flagged by the model | ≥ 85% | Quarterly | < 80% triggers investigation |
| **Hold resolution time** | Average time from hold applied to analyst decision | ≤ 4 business hours | Monthly | > 6 hours average triggers capacity review |
| **Score distribution stability** | KS test on fraud score distribution vs. baseline | p > 0.05 | Monthly | p < 0.05 for two consecutive months |

### 4.2 Fairness Metrics

| Metric | Definition | Target | Frequency | Alert Threshold |
|---|---|---|---|---|
| **Hold rate by demographic proxy** | Rate of holds applied across geographic and demographic proxy groups | No statistically significant disparity | Quarterly | Significant disparity triggers bias investigation |
| **Demographic parity ratio (hold rates)** | Hold rate for flagged groups vs. overall customer population | ≥ 0.80 | Quarterly | < 0.80 triggers immediate AGPO escalation |
| **Bias review completion** | Formal bias assessment of NP-002 geographic features | Complete - Q4 2026 | One-time (then annual) | Overdue triggers escalation to AI Governance Committee |

> **Note:** The full NP-002 fairness monitoring framework will be finalised following completion of the Q4 2026 bias review. The metrics above represent the interim approach. Baselines and refined thresholds will be defined in a document update upon review completion.

### 4.3 NP-002 Revalidation Triggers

| Trigger | Action |
|---|---|
| False positive rate > 0.5% for two consecutive months | Threshold recalibration review |
| Detection rate < 80% for one quarter | Immediate investigation; possible emergency retraining |
| Bias review identifies demographic disparity above threshold | Containment and remediation following Phase 4 incident response model |
| Significant new fraud typology identified by Financial Crime intelligence | Targeted retraining |
| 24 months since last full revalidation | Scheduled revalidation |

---

## 5. Monitoring Cadence and Responsibilities

| Activity | Frequency | Owner | Output |
|---|---|---|---|
| Automated metric dashboard refresh | Daily | IT and AGPO (automated) | Dashboard accessible to system owners and AGPO |
| Override rate review (NP-001) | Monthly | Head of Credit Risk | Escalation to AGPO if threshold triggered |
| False positive rate review (NP-002) | Monthly | Head of Financial Crime | Escalation to AGPO if threshold triggered |
| Fairness analysis - both systems | Quarterly | AGPO and ML Engineering | Included in quarterly AI Governance Committee report |
| Full performance report - both systems | Quarterly | AGPO | Report to AI Governance Committee |
| Annual system review - both systems | Annual | AGPO | Full review; continuation decision |

---

## 6. Serious Incident Escalation

If post-deployment monitoring detects a potential serious incident, the following escalation applies:

```
[Detection] Metric alert triggered or complaint received
     ↓
[AGPO triage] Is this a potential serious incident? - within 24h
     ↓
No → Document finding; increase monitoring frequency for affected metric
     ↓
Yes → Initiate AI Incident Response per Phase 4 procedure
     ↓
     AI Governance Committee notified within 24h
     ↓
     CRO notified within 24h
     ↓
     Assess regulatory reporting obligation - EU AI Act Article 73, FCA, ICO
     ↓
     Containment, investigation and remediation
```

Performance monitoring is the primary mechanism by which serious incidents are detected before they escalate. The Phase 4 incident showed what happens when fairness monitoring is absent. This escalation path is designed to ensure that a detected issue reaches the right decision-makers before harm accumulates.

---

## 7. Annual System Review

At the 12-month mark from deployment of each version - July 2027 for both systems under this documentation version - AGPO will conduct a full annual review covering:

1. Performance summary against all metrics in this plan
2. Fairness analysis summary
3. Incident log review
4. Oversight quality assessment - override rates, review time, challenge rates
5. Regulatory change assessment - any new obligations that affect either system
6. Recommendation: continue as-is / modify controls / decommission

The annual review is presented to the AI Governance Committee. Continuation in production is conditional on a satisfactory review and committee sign-off.

All monitoring outputs - dashboards, quarterly reports, annual reviews, incident records and revalidation findings - are retained for 7 years in the AI Governance Programme Office documentation archive and are available for regulatory inspection.

---

*This monitoring plan is a living document. Metric thresholds and review frequencies may be adjusted based on operational experience, subject to AGPO approval and AI Governance Committee notification.*
