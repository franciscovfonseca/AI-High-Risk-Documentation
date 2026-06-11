# Document 1: System Description and Intended Purpose

**Systems:** NP-001 Credit Scoring Engine · NP-002 Fraud Detection System  
**Provider and Deployer:** NorthPoint Financial Services (internally developed and operated)  
**EU AI Act Reference:** Article 11 · Annex IV §1 and §2  
**Version:** 1.0 - July 2026  
**Prepared by:** AI Governance Programme Office and ML Engineering  

---

## Provider/Deployer Note

NorthPoint Financial Services is both the developer and the operator of NP-001 and NP-002. Both systems were built by NorthPoint's ML Engineering team and are operated by NorthPoint business divisions. This means NorthPoint holds the obligations of both provider and deployer under the EU AI Act. This document fulfils the Annex IV §1 and §2 requirements for both systems and is addressed to the AI Governance Committee, regulators and auditors.

---

## NP-001 - Credit Scoring Engine

### 1. System Identification

| Field | Value |
|---|---|
| System name | Credit Scoring Engine |
| System ID | NP-001 |
| Version documented | v2.0 |
| Developer | NorthPoint ML Engineering |
| Operator | NorthPoint Financial Services (Credit Risk division) |
| Deployment context | Personal loan origination - all retail and digital channels |
| EU AI Act classification | HIGH RISK - Annex III §5(b) |
| Original deployment | March 2022 (v1.0) |
| Current version deployed | June 2026 (v2.0 - post-incident remediation) |

> **Version context:** v2.0 is the retrained model deployed following the NP-INC-2026-001 bias incident documented in Phase 4. The postcode geographic feature was removed, the model was retrained on data from 2015-2024 and independently validated before redeployment in June 2026. This documentation covers v2.0 only. v1.0 is decommissioned and must not be used for active lending decisions.

### 2. General Description

NP-001 is an ML-based credit scoring model that estimates the probability of default for personal loan applicants. It uses a gradient boosting architecture trained on NorthPoint's historical lending portfolio.

The system processes applicant data submitted at the point of loan application and produces:

- A credit score on a 0-1000 scale
- A default probability estimate
- A risk tier classification: Low / Medium / High / Decline
- A feature contribution summary identifying the five principal drivers of the score

NP-001's output informs, but does not autonomously determine, credit decisions. Final decisions are made by loan underwriters in the Credit Risk division who use the score as one structured input alongside other assessment criteria.

### 3. Intended Purpose

**Primary purpose:** To assist NorthPoint's credit risk underwriting team in assessing the creditworthiness of personal loan applicants, supporting consistent and evidence-based lending decisions at scale.

**Intended users:** Trained loan underwriters in NorthPoint's Credit Risk division.

**Intended context of use:** All personal loan applications submitted through NorthPoint's retail and digital channels in the UK. The system is not intended for and must not be used for, business lending, mortgage assessment, internal employee credit products or any application category outside the standard personal loan origination process.

**How outputs are intended to be used:**
- Credit score and risk tier are structured inputs for the underwriter - not autonomous decisions
- Underwriters are required to review the feature contribution summary and exercise independent judgment
- Decline decisions based solely on NP-001 output without underwriter review are prohibited

### 4. Prohibited Uses

- Using NP-001 output as the sole basis for a lending decline without underwriter review
- Applying NP-001 to application types outside personal lending
- Retraining or modifying the model without AGPO approval and independent validation
- Operating v1.0 of the model for any active lending decision
- Operating NP-001 without the fairness monitoring controls mandated following the Phase 4 incident

### 5. Known Limitations

| Limitation | Description | Mitigation |
|---|---|---|
| Thin-file applicants | Performance is less reliable for applicants with limited credit history. Scores carry higher uncertainty. | Feature contribution summary flags thin-file status. Underwriters receive specific guidance for these cases. |
| Population shift | The model was trained on 2015-2024 data. Significant macroeconomic changes may reduce predictive accuracy. | Annual model revalidation; off-cycle review triggered by economic indicators. |
| Residual proxy risk | Some proxy discrimination risk exists in any socioeconomic feature. The postcode variable was removed in v2.0; other features are reviewed annually. | Ongoing demographic parity monitoring; annual proxy analysis of all features. |
| Explainability scope | The feature contribution summary covers the top 5 contributors. Interaction effects between features are not captured. | Underwriters trained on the limitations of the explanation output. Full SHAP values available to AGPO for investigation purposes. |

### 6. EU AI Act Classification

**Classification:** HIGH RISK
**Legal basis:** EU AI Act Annex III §5(b) - AI systems used in financial services that determine or materially influence access to credit.

NP-001 directly informs whether individuals are granted or denied access to personal credit. The consequential nature of the decisions it informs and the regulatory context of financial services lending make this classification clear and non-controversial.

**Conformity assessment pathway:** Internal control under Article 43(2). NP-001 is not in a product category requiring third-party conformity assessment.

---

## NP-002 - Fraud Detection System

### 1. System Identification

| Field | Value |
|---|---|
| System name | Fraud Detection System |
| System ID | NP-002 |
| Version documented | v1.4 |
| Developer | NorthPoint ML Engineering |
| Operator | NorthPoint Financial Services (Financial Crime division) |
| Deployment context | Real-time transaction monitoring - all retail and digital transaction channels |
| EU AI Act classification | HIGH RISK - Annex III §5(b) |
| Original deployment | September 2021 (v1.0) |
| Current version deployed | November 2024 (v1.4) |

### 2. General Description

NP-002 is a real-time transaction fraud detection system that monitors customer transactions as they occur and flags those it assesses as potentially fraudulent. It uses a gradient boosting model trained on NorthPoint's historical transaction data and confirmed fraud case records.

The system produces:

- A fraud probability score for each transaction (0-100)
- A binary recommendation: pass or flag
- A reason code indicating the primary fraud signal or signals detected

For transactions flagged at or above the automated hold threshold (score ≥ 85), the system places an automatic temporary hold on the transaction pending human review by a fraud analyst. Transactions scoring below the threshold pass through without hold.

### 3. Intended Purpose

**Primary purpose:** To detect fraudulent transactions in real time, protecting NorthPoint customers from financial harm while minimising disruption to legitimate transaction activity.

**Intended users:** Fraud analysts in NorthPoint's Financial Crime division. The automated hold mechanism also operates without direct user intervention for transactions above the threshold.

**Intended context of use:** All retail and digital transactions processed through NorthPoint's UK systems. The system operates continuously. Automated holds are applied 24/7; human review is conducted during business hours with a 4-hour resolution target.

**How outputs are intended to be used:**
- Pass/flag recommendation informs the hold decision
- Reason code is used by fraud analysts to assess flagged transactions
- All automated holds above the 85-threshold are reviewed by a fraud analyst before any permanent account action

### 4. Prohibited Uses

- Using NP-002 output as the sole basis for permanent account suspension without human investigation
- Applying NP-002 thresholds to transaction types outside the standard retail and digital transaction scope
- Modifying the automated hold threshold without AGPO and Financial Crime Director approval
- Using NP-002 output for credit risk assessment

### 5. Known Limitations

| Limitation | Description | Mitigation |
|---|---|---|
| False positive rate | The model generates false positives at approximately 0.4% of flagged transactions at the current threshold. | Fraud analysts review all holds within 4 business hours. False positive SLA: < 0.5%. |
| Novel fraud patterns | The model is trained on historical patterns. Genuinely novel techniques may evade detection until retraining. | Monthly fraud intelligence review. Retraining triggered by emerging pattern detection. |
| Transaction context limitations | NP-002 assesses individual transactions and has limited visibility of cross-account or cross-customer patterns. | Fraud analysts supplement NP-002 output with broader investigation tools for complex cases. |
| Demographic disparity risk | NP-002 uses postcode as a geographic risk variable - the same category of feature that produced bias in NP-001. A bias review was initiated in March 2026 following the Phase 4 parallel review trigger. | Enhanced monitoring of hold rates by demographic proxy. Full bias review target: Q4 2026. |

### 6. EU AI Act Classification

**Classification:** HIGH RISK
**Legal basis:** EU AI Act Annex III §5(b) - AI systems used in financial services that determine or materially influence access to financial resources.

NP-002 places automatic holds on customer transactions and funds. The material impact on customers' ability to access their money and the financial services context make this classification clear.

**Conformity assessment pathway:** Internal control under Article 43(2).

---

*Prepared by the AI Governance Programme Office - July 2026. This document forms part of the Annex IV technical documentation pack for NP-001 and NP-002. Distribution: AI Governance Committee · FCA (on request) · ICO (on request) · Internal Audit.*
