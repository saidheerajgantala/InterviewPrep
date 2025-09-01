# OAuth 2.0 and OpenID Connect

## 0) Metadata
- **Name**: OAuth 2.0 and OpenID Connect
- **Canonical Path**: Patterns/012_Security/AuthenticationAuthorization/OAuth_OpenID.md
- **Category**: 012 Security / Authentication & Authorization
- **Status**: Stable
- **Last Updated**: YYYY-MM-DD
- **Tags**: oauth2, oidc, tokens, scopes, consent

---

## 1) TL;DR (Executive Summary)
- OAuth 2.0: delegated authorization via access tokens. OIDC: identity layer on top providing ID tokens.

---

## 2) Flows
- Authorization Code (with PKCE for public clients), Client Credentials, Refresh Tokens.
- Implicit (legacy, avoid), Device Code for TVs/CLI.

## 3) Tokens
- Access tokens (opaque or JWT); scopes; audience; expiry; rotation.
- ID tokens (JWT) with claims; validate signature/issuer/audience.

---

## 4) Implementation Guide
- Use well-known providers; rotate keys (JWKS); validate tokens on each request.
- Least privilege scopes; consent screens; secure redirect URIs.

---

## 5) Pitfalls & Edge Cases
- Token leakage in URLs; use form_post or code flow.
- Long-lived tokens; prefer short-lived with refresh tokens.

---

## 6) References
- RFC 6749/8252/7636; OpenID Connect Core; security best practices.
