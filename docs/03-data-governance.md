# Document 3: Data Governance

**Systems:** NP-001 Credit Scoring Engine · NP-002 Fraud Detection System  
**EU AI Act Reference:** Article 10 (Data and Data Governance)  
**Version:** 1.0 - July 2026  
**Prepared by:** AI Governance Programme Office and ML Engineering  

---

## 1. Overview

Article 10 requires that high-risk AI systems use training, validation and testing data that meets defined quality criteria - including relevance, representativeness, completeness and freedom from errors. This document records the data governance practices for both NP-001 and NP-002, covering training data used to develop each system and the operational customer data processed during deployment.

---

## 2. NP-001 - Credit Scoring Engine

### 2.1 Training Data

| Parameter | Detail |
|---|---|
| Dataset | NorthPoint Historical Lending Portfolio |
| Size | 1.4 million loan applications |
| Geographic coverage | UK - all NorthPoint retail lending regions |
| Time range | 2015-2024 (v2.0) |
| Source | NorthPoint internal loan origination system |
| Outcome variable | Default / no-default over a 24-month outcome window |
| Key change from v1.0 | Postcode feature removed following Phase 4 incident |

### 2.2 Data Relevance and Representativeness

| Criterion | Assessment |
|---|---|
| **Relevance** | Dataset consists of NorthPoint's own historical lending decisions and outcomes - directly relevant to creditworthiness prediction for NorthPoint's customer base. ✓ |
| **Representativeness** | Covers NorthPoint's full UK retail lending geography. Following Phase 4, representativeness by demographic proxy has been assessed. No statistically significant underrepresentation identified after postcode removal. |
| **Completeness** | 1.4 million records with full outcome data. Assessed as sufficient for the task. |
| **Freedom from errors** | Automated checks applied: duplicate removal, incomplete record filtering, outcome labelling validation. 0.3% record exclusion rate. |

### 2.3 Bias Assessment - Training Data (v2.0)

An independent fairness evaluation was conducted on the v2.0 training dataset before redeployment. All measurable protected characteristics meet NorthPoint's fairness threshold of ≥ 0.80 demographic parity ratio.

| Protected Characteristic | Proxy Indicator | Demographic Parity Ratio | Status |
|---|---|---|---|
| Race / Ethnicity | Name-based inference | 0.94 | Within threshold ✓ |
| Gender | Name-based inference | 0.97 | Within threshold ✓ |
| Age | Age band derived from date of birth | 0.91 | Within threshold ✓ |
| Disability | No reliable proxy available | Not measurable | Manual review pathway in place |

### 2.4 Historical Data Encoding Risk

A key finding from the Phase 4 root cause analysis was that training on historical lending data can encode patterns reflecting past discriminatory practices - even without an explicit discriminatory feature. NorthPoint's lending data from 2015-2021 reflects decisions made before the governance programme was in place.

Controls applied in v2.0 training to address this:
- Outcome reweighting applied to partially correct for historical decision bias in the 2015-2021 window
- 2022-2024 data - post-governance programme - weighted more heavily in the training split
- Annual review of training data composition required as a standing governance obligation

---

## 3. NP-002 - Fraud Detection System

### 3.1 Training Data

| Parameter | Detail |
|---|---|
| Dataset | NorthPoint Transaction and Fraud History |
| Size | 86 million transactions |
| Fraud label source | Confirmed fraud cases from Financial Crime investigation records |
| Time range | 2019-2024 |
| Fraud prevalence in training data | 0.06% |
| Class imbalance treatment | SMOTE oversampling applied during training |

### 3.2 Data Relevance and Representativeness

| Criterion | Assessment |
|---|---|
| **Relevance** | NorthPoint's own transaction data - directly relevant to detecting fraud patterns in NorthPoint's customer base. ✓ |
| **Representativeness** | Covers all NorthPoint transaction channels and customer segments. Label quality depends on Financial Crime investigation completeness - a known limitation. |
| **Class imbalance** | Fraud prevalence of 0.06% requires oversampling. SMOTE applied. |
| **Label quality** | Derived from confirmed investigation outcomes. Borderline unconfirmed cases excluded from training data to avoid label noise. |

### 3.3 Bias Assessment - Training Data (Interim)

A formal bias assessment of the NP-002 training data is part of the ongoing bias review initiated in Phase 4 (target: Q4 2026). Preliminary findings are recorded here; this section will be updated on completion of the full review.

| Characteristic | Proxy | Preliminary Status |
|---|---|---|
| Geography (postcode) | Direct feature in the model | Under review - same risk category as the NP-001 feature removed in Phase 4 |
| Age | Transaction behaviour patterns | No significant disparity identified in preliminary analysis |
| Transaction value | Indirect income/wealth proxy | Flagged as a monitoring priority |

---

## 4. Operational Data Processing

### 4.1 NP-001 - Applicant Data Processed at Runtime

| Data Category | Source | Processing Purpose | Retention |
|---|---|---|---|
| Loan application data (income, employment, credit history) | Applicant submission | Model input for credit scoring | 7 years |
| Credit bureau data | Experian and Equifax (with applicant consent) | Model input | 7 years |
| Model output (score, risk tier, feature contributions) | Generated by NP-001 | Underwriter decision support | 7 years |
| Underwriter decision and override notes | Credit Risk team | Decision record and audit trail | 7 years |

### 4.2 NP-002 - Transaction Data Processed at Runtime

| Data Category | Source | Processing Purpose | Retention |
|---|---|---|---|
| Transaction data (amount, merchant, channel, timestamp) | NorthPoint transaction systems | Real-time model input | 7 years |
| Fraud score and reason code | Generated by NP-002 | Fraud analyst review | 7 years |
| Hold decision and analyst outcome | Financial Crime team | Investigation record | 7 years |
| Customer dispute record | Customer contact | Resolution and audit trail | 7 years |

**Retention basis:** 7 years is the FCA record-keeping standard for retail financial services activities and covers UK Equality Act and GDPR discrimination claim limitation periods.

### 4.3 Lawful Basis

| System | Processing Activity | Lawful Basis |
|---|---|---|
| NP-001 | Creditworthiness assessment | Legitimate interests (UK GDPR Article 6(1)(f)) - or contract performance for existing customers (Article 6(1)(b)); legal to confirm preferred basis per application type |
| NP-001 | Logging of AI-assisted decisions | Legal obligation (Article 6(1)(c)) - EU AI Act Article 12 requires logging |
| NP-002 | Fraud detection processing | Legitimate interests (Article 6(1)(f)) - fraud prevention is a recognised legitimate interest in financial services |
| NP-002 | Account hold and investigation | Legitimate interests / Legal obligation - financial crime prevention |

### 4.4 GDPR Article 22 Obligations

Both NP-001 (credit decisions) and NP-002 (account holds) involve automated processing with significant effects on individuals. NorthPoint's obligations under UK GDPR Article 22 are addressed as follows:

- Customers are informed at the point of application or account onboarding that AI is used in credit assessment and fraud detection
- All individuals subject to an AI-influenced decision have the right to request human review
- Decline communications (NP-001) and hold notifications (NP-002) include a GDPR Article 22 notice and clear instructions for exercising the right to human review
- GDPR notices reviewed and approved by the DPO - July 2026

### 4.5 Special Category Data

CVs or application forms may inadvertently contain special category data (health information or other sensitive characteristics). Both systems include input validation filters to detect and flag records that may contain such information. Flagged records are routed for manual review only. Applicants and customers are instructed through onboarding communications not to include sensitive personal information beyond what is relevant to their application.

---

## 5. Data Governance Controls

| Control | Owner | Status |
|---|---|---|
| Annual proxy analysis - all features for NP-001 | ML Engineering and AGPO | In place - next review June 2027 |
| NP-002 bias review - postcode and geographic features | ML Engineering and AGPO | In progress - Q4 2026 |
| Customer-facing AI disclosure - NP-001 and NP-002 | Legal and DPO | In place ✓ |
| GDPR Article 22 notices - NP-001 and NP-002 | Legal and DPO | In place - July 2026 ✓ |
| Data retention enforcement at 7-year threshold | IT and DPO | Automated deletion in place ✓ |
| Annual data governance review | DPO and AGPO | Included in annual system review cycle |
| Training data composition review (NP-001 v2.0) | ML Engineering | Complete for v2.0 - next review at next major retraining |

---

*Data governance is a continuous obligation. This document is updated following retraining, material data governance changes or annual review.*
