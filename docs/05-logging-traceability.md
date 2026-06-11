# Document 5: Logging and Traceability

**Systems:** NP-001 Credit Scoring Engine · NP-002 Fraud Detection System  
**EU AI Act Reference:** Article 12 (Record-Keeping)  
**Version:** 1.0 - July 2026  
**Prepared by:** AI Governance Programme Office and Information Security  

---

## 1. Purpose

Article 12 requires that high-risk AI systems have logging capabilities that enable traceability of the system's operation. Logs must enable post-hoc verification that the system operated as intended, identification of the circumstances of use, detection and investigation of incidents and evidence for compliance audits and regulatory review.

This document describes the logging architecture for NP-001 and NP-002.

> **Phase 4 context:** The investigation into NP-INC-2026-001 revealed that the absence of structured logging significantly increased investigation time. Reconstructing four years of decisions without a formal audit trail required weeks of manual data recovery. The logging architecture described here is designed to ensure that any future investigation can proceed without that impediment.

---

## 2. Events Logged

### NP-001 - Credit Scoring Engine

| Event Category | Specific Events |
|---|---|
| **System inputs** | Application ID; applicant data hash; credit bureau query reference; submission timestamp; channel |
| **Model outputs** | Credit score; risk tier; default probability; feature contribution summary (top 5); model version; inference timestamp |
| **Underwriter actions** | Review initiated; reviewer ID; decision recorded; override indicator; override reason text; decision timestamp; time spent on review |
| **Customer communications** | Communication type (approval/decline); timestamp; communication reference |
| **Overrides** | Override initiated; original AI recommendation; revised decision; reason text; reviewer ID |
| **Model version** | Version number at time of each inference - enables correlation of decisions to specific model releases |
| **System exceptions** | Low-confidence flag triggered; input validation failures; credit bureau query failures; system errors |
| **Access and authentication** | User login and logout; role; session duration |

### NP-002 - Fraud Detection System

| Event Category | Specific Events |
|---|---|
| **System inputs** | Transaction ID; transaction data hash; processing timestamp; channel |
| **Model outputs** | Fraud score; pass/flag recommendation; reason code; model version; inference timestamp |
| **Automatic hold events** | Hold applied; hold threshold at time of decision; customer notification sent; notification timestamp |
| **Analyst actions** | Review initiated; analyst ID; hold decision (cleared/confirmed); decision reasoning; resolution timestamp; time spent |
| **Customer disputes** | Dispute received; timestamp; resolution outcome; resolution timestamp; compensation record (if applicable) |
| **Model version** | Version number at time of each inference |
| **System exceptions** | Threshold breach events; fallback mode activation; system errors |
| **Access and authentication** | User login and logout; role; session duration |

---

## 3. Log Architecture

### 3.1 Log Storage

| Log Type | Location | Retention | Access Controls |
|---|---|---|---|
| NP-001 decision audit logs | NorthPoint secure data warehouse (UK-hosted, ISO 27001 certified) | 7 years | AGPO · Legal · DPO · Internal Audit (read); automated processes only (write) |
| NP-002 transaction audit logs | NorthPoint secure data warehouse (UK-hosted, ISO 27001 certified) | 7 years | AGPO · Legal · DPO · Information Security · Internal Audit (read); automated processes only (write) |
| Model version history | ML Engineering source control (Git, UK-hosted) | Indefinite | ML Engineering · AGPO |

**Retention basis:** 7 years covers the FCA record-keeping standard for retail financial services activities, UK Equality Act discrimination claim limitation periods and GDPR enforcement timelines.

### 3.2 Log Integrity Controls

| Control | Implementation |
|---|---|
| Log immutability | Append-only storage; modification requires CISO-level approval and creates an audit entry |
| Encryption at rest | AES-256 for all stored logs |
| Hash-chaining | Applied to model output and decision log entries - enables tamper detection |
| Access audit | All log access events are themselves logged with user ID and timestamp |
| Backup | Daily backup to a geographically separate UK data centre |
| Regulatory access | Full log set can be produced for regulatory inspection within 24 hours of a formal request |

---

## 4. Traceability Use Cases

### Tracing a single credit decision (NP-001)

For any loan applicant, the complete decision chain can be reconstructed from logs:

```
Applicant X submits application [timestamp]
  → NP-001 v2.0 generates: score 634, tier: Medium, features: [income ratio 0.31,
    credit history age 7yr, utilisation rate 0.42, employment tenure 3yr, debt-to-income 0.28]
    [inference timestamp]
  → Underwriter A reviews for 9 minutes [timestamp]
  → Override: underwriter declines application
    Reason: "Significant recent credit enquiries not reflected in bureau snapshot - likely
    undisclosed liabilities pending settlement. Manual review confirmed."
  → Decline communication sent with GDPR Article 22 notice [timestamp]
  → Log entry: override recorded, reason stored, reviewer ID recorded
```

This chain supports: regulatory audit of AI involvement, customer right-to-explanation requests and oversight quality monitoring.

---

### Tracing a transaction hold (NP-002)

For any held transaction, the full event chain is recoverable:

```
Transaction T processed [timestamp]
  → NP-002 v1.4 generates: score 91, flag, reason: "Unusual merchant category +
    geographic anomaly - transaction location inconsistent with 90-day customer pattern"
  → Automatic hold applied; customer notified via app and SMS [timestamp]
  → Fraud Analyst B reviews for 14 minutes [timestamp]
  → Hold cleared: "Contacted customer - confirmed overseas travel. Transaction verified
    as legitimate. Customer account flagged for travel window."
  → Transaction released; customer notified [timestamp]
```

This chain supports: FCA Consumer Duty audit, false positive rate monitoring and customer dispute resolution.

---

### Post-incident bias investigation (both systems)

Following a potential bias incident, the log structure enables:

- Extraction of all decisions within a defined time period for a demographic segment or postcode cluster
- Cross-referencing of AI recommendation versus human decision to identify override patterns by group
- Model version identification to correlate potential issues with specific model releases
- Timeline reconstruction suitable for regulatory reporting under EU AI Act Article 73

---

## 5. Log Review and Active Monitoring

Logs are actively monitored - not merely stored - as part of the ongoing governance of both systems.

| Activity | Frequency | Owner |
|---|---|---|
| Override rate monitoring (NP-001) | Monthly | Head of Credit Risk |
| False positive rate monitoring (NP-002) | Monthly | Head of Financial Crime |
| Review time analysis - oversight effectiveness (both) | Monthly | AGPO |
| Demographic parity analysis - proxy group outcomes (both) | Quarterly | AGPO and ML Engineering |
| Access audit review | Quarterly | Information Security |
| Full log audit | Annual | Internal Audit |

Anomalies detected through log monitoring trigger escalation to AGPO and, if material, initiation of an AI incident review following the Phase 4 incident response process.

---

## 6. Known Limitations

| Limitation | Mitigation |
|---|---|
| Demographic data availability | Applicants and customers are not required to disclose protected characteristics. Demographic parity analysis relies on proxy indicators which are imperfect. This limitation is documented in each monitoring report. |
| Feature contribution logging covers top 5 contributors only | Full SHAP values are available to AGPO for investigation purposes; only summary values are stored in the standard audit log to manage storage volume. |
| NP-002 bias review pending | Until the Q4 2026 bias review is complete, demographic log analysis for NP-002 uses interim proxy methods. Results are flagged as preliminary in monitoring reports. |

---

*Log architecture and retention schedules are reviewed annually. Changes to log scope or retention periods require AGPO approval.*
