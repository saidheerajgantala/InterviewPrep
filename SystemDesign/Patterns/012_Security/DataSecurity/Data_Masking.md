# Data Masking

## 0) Metadata
- **Name**: Data Masking
- **Canonical Path**: Patterns/012_Security/DataSecurity/Data_Masking.md
- **Category**: 012 Security / Data Security
- **Status**: Stable
- **Last Updated**: YYYY-MM-DD
- **Tags**: masking, pii, anonymization, tokenization

---

## 1) TL;DR (Executive Summary)
- Protect sensitive fields by masking/anonymizing/tokenizing in non-prod and when not strictly needed in prod.

---

## 2) Techniques
- Static masking (ETL); dynamic masking (at query time); tokenization; format-preserving encryption.

## 3) Implementation Guide
- Classify fields; policies by role/environment; mask logs/exports.
- Reversible vs irreversible (pseudonymization vs anonymization) based on use case.

---

## 4) Pitfalls
- Re-identification via joins; minimize linkage.
- Leaks in logs; ensure structured logging and scrubbing.

---

## 5) References
- GDPR/CCPA guidance; vendor docs; privacy engineering.
