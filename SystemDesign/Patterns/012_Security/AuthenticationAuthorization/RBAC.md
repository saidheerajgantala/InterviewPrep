# Role-Based Access Control (RBAC)

## 0) Metadata
- **Name**: RBAC
- **Canonical Path**: Patterns/012_Security/AuthenticationAuthorization/RBAC.md
- **Category**: 012 Security / Authentication & Authorization
- **Status**: Stable
- **Last Updated**: YYYY-MM-DD
- **Tags**: rbac, roles, permissions, policy

---

## 1) TL;DR (Executive Summary)
- Assign permissions to roles; assign roles to principals (users/services) for manageable authorization.

---

## 2) Concepts
- Roles, permissions, role hierarchies; least privilege.
- ABAC (attributes) and ReBAC (relationships) as alternatives/adjuncts.

## 3) Implementation Guide
- Define permission taxonomy; align to APIs/actions.
- Policy store/service; decision point at gateway/service.
- Admin UI for role management; audit logs for changes.

---

## 4) Pitfalls & Edge Cases
- Role explosion; adopt hierarchies or ABAC claims where needed.
- Stale permissions; periodic reviews.

---

## 5) References
- NIST RBAC model; policy engines (OPA), AuthZ frameworks.
