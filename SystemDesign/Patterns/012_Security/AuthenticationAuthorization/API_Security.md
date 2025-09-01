# API Security

## 0) Metadata
- **Name**: API Security
- **Canonical Path**: Patterns/012_Security/AuthenticationAuthorization/API_Security.md
- **Category**: 012 Security / Authentication & Authorization
- **Status**: Stable
- **Last Updated**: YYYY-MM-DD
- **Tags**: oauth2, tls, input-validation, rate-limit, mTLS, WAF

---

## 1) TL;DR (Executive Summary)
- Enforce TLS, authenticate/authorize every call, validate inputs, protect against abuse, and log with traceability.

---

## 2) Controls
- TLS/mTLS; OAuth2/OIDC; RBAC/ABAC; rate limiting; WAF; input validation; output encoding; secrets management.

## 3) Implementation Guide
- Centralize at gateway; policy-as-code; consistent error handling.
- Threat modeling for APIs; least privilege scopes; audit logging.

---

## 4) Pitfalls & Edge Cases
- Missing auth on internal paths; treat internal as hostile.
- Inconsistent validation; shared libraries/schemas.

---

## 5) References
- OWASP API Security Top 10; NIST; vendor best practices.
