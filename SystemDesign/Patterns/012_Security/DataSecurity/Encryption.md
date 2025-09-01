# Encryption (At Rest & In Transit)

## 0) Metadata
- **Name**: Encryption
- **Canonical Path**: Patterns/012_Security/DataSecurity/Encryption.md
- **Category**: 012 Security / Data Security
- **Status**: Stable
- **Last Updated**: YYYY-MM-DD
- **Tags**: tls, kms, tde, key-rotation

---

## 1) TL;DR (Executive Summary)
- TLS in transit; TDE/field-level encryption at rest; centralized key management with rotation.

---

## 2) Concepts
- TLS 1.2+; strong ciphers; cert rotation/automation.
- TDE vs application-level encryption; envelope encryption; KMS/HSM.

## 3) Implementation Guide
- Enforce TLS; mTLS internally; rotate certs/keys; audit key access.
- Classify data; encrypt PII; tokenization where needed.

---

## 4) Pitfalls & Edge Cases
- Custom crypto; use vetted libraries.
- Key leakage in logs/backups; scrub and control access.

---

## 5) References
- NIST, OWASP Cryptographic Storage Cheat Sheet; cloud KMS docs.
