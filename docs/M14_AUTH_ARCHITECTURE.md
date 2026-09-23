# M14 ENTERPRISE AUTHENTICATION & RBAC ARCHITECTURE

## Executive Summary
Milestone 14 upgrades the authentication architecture from prototype admin secret key verification to a full standards-compliant **Role-Based Access Control (RBAC)** model supporting OIDC / OAuth2 identity integration without sacrificing local SIH demonstration simplicity.

---

## Deployment Profiles

1. **`DEMO_LOCAL` (Default for SIH Demo)**:
   - Accepts controlled local tokens (`demo-admin-token`, `demo-analyst-token`) to ensure single-command demo execution (`scripts/start_sih_demo.py`) works out of the box without external Keycloak/IdP dependencies.
2. **`SECURE_LOCAL`**:
   - Uses local HMAC-SHA256 signed JWTs with explicit expiration and issuer validation.
3. **`ENTERPRISE`**:
   - Integrates with standard OIDC/OAuth2 providers (Keycloak, Supabase Auth, Auth0) verifying RS256/ES256 public key signatures, issuer claims, and audience limits.

---

## Role & Capability Matrix

| Role | Operational Scope | Permitted Endpoints | Denied Endpoints |
|---|---|---|---|
| **`USER`** | Current call session only | `/api/v1/audio/analyze`, `/api/v1/stream/ws` | `/api/v1/admin/*`, `/api/v1/cases/*`, `/api/v1/forensics/*` |
| **`ANALYST`** | Assigned cases & evidence | `/api/v1/cases/*`, `/api/v1/forensics/*`, `/api/v1/drift/*` | Direct server maintenance routes |
| **`ADMIN`** | System management & exports | All `/api/v1/admin/*`, `/api/v1/cases/*`, `/api/v1/forensics/*` | None |
| **`AUDITOR`** | Read-only compliance audit | `/api/v1/admin/audit_logs`, `/api/v1/cases` (read-only) | Case modification, system maintenance |

---

## Server-Side Security Invariants
- **Zero Frontend Role Trust**: User roles are verified exclusively on the server side via token claim evaluation. Client-side state flags are ignored.
- **Exfiltration Defense**: Ordinary `USER` role tokens attempting to access `/api/v1/admin/*` or `/api/v1/cases/*` receive immediate HTTP `403 Forbidden` responses.
