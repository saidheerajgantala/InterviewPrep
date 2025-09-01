# JSON Web Tokens (JWT)

## 0) Metadata
- **Name**: JWT
- **Canonical Path**: Patterns/012_Security/AuthenticationAuthorization/JWT.md
- **Category**: 012 Security / Authentication & Authorization
- **Status**: Stable
- **Last Updated**: YYYY-MM-DD
- **Tags**: jwt, tokens, signing, claims

---

## 1) TL;DR (Executive Summary)
- Self-contained tokens with signed claims; useful for stateless authn/z when validated correctly.

---

## 2) Concepts
- Header.payload.signature; JWS (signed) vs JWE (encrypted).
- Claims: iss, sub, aud, exp, iat, nbf; custom claims.
- Sign with asymmetric keys (RS256/ES256) preferred; rotate keys (JWKS).

## 3) Implementation Guide
- Validate signature, issuer, audience, expiry; check clock skew.
- Keep tokens short-lived; use refresh tokens when needed.
- Avoid putting sensitive data in JWT; prefer opaque tokens if revocation required.

---

## 4) Pitfalls & Edge Cases
- Algorithm confusion; enforce expected alg.
- Token replay; use jti + store if revocation required.

---

## 5) References
- RFC 7519; OWASP JWT cheatsheet.
