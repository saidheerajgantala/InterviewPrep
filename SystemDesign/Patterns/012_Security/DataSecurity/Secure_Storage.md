# Secure Storage

## 0) Metadata
- **Name**: Secure Storage
- **Canonical Path**: Patterns/012_Security/DataSecurity/Secure_Storage.md
- **Category**: 012 Security / Data Security
- **Status**: Stable
- **Last Updated**: YYYY-MM-DD
- **Tags**: secrets, kms, vault, access-control

---

## 1) TL;DR (Executive Summary)
- Store secrets and sensitive data in dedicated secure stores with strict access controls and auditing.

---

## 2) Concepts
- Secret stores (Vault, cloud Secrets Manager), KMS for encryption keys, HSMs for high security.
- Least privilege, rotation, versioning, audit trails.

## 3) Implementation Guide
- Centralize secret management; remove secrets from code/images.
- Use IAM roles/Workload Identity; short-lived credentials.

---

## 4) Pitfalls
- Secrets in environment variables and logs; rotate on exposure.

---

## 5) References
- Vault/Secrets Manager docs; CIS benchmarks.
