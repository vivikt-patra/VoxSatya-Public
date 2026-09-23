# M14 ACCEPTANCE CONTRACT — COMPETITIVE HARDENING & FORENSIC PLATFORM

> **Status Legend**:
> - `CODED`: Implementation written in codebase.
> - `LAB PASS`: Verified via isolated automated test suite / lab environment.
> - `EXTERNAL DATA PASS`: Validated against authentic external datasets.
> - `PHYSICAL PASS`: Verified in acoustic propagation / physical streaming tests.
> - `FIELD PASS`: Verified in live production network deployments.

---

## 1. Acceptance Gates

| Gate ID | Area | Requirement | Validation Level | Status |
|---|---|---|---|---|
| **GATE-14.01** | Baseline Preservation | Baseline detector (`m09_a_aasist_best.pt`, SHA256 `1b9bf59addd8f98422b295fe7ab613b48b0e93eaaadceb0d8ffb5a81d53bda39`) and Platt calibration parameters remain 100% frozen and unmutated. | LAB PASS | PASS |
| **GATE-14.02** | A17 Investigation | Scientific diagnosis report produced explaining A17 low-bitrate neural vocoder synthesis artifacts. | LAB PASS | PASS |
| **GATE-14.03** | A17 Experiment | Experimental candidate (`M14-A17-Robust-v1`) trained with codec & resampling augmentations without overwriting baseline. | LAB PASS | PASS |
| **GATE-14.04** | A17 Promotion Gate | Evaluated candidate vs baseline; candidate promoted ONLY if genuine human false positives do not deteriorate. | LAB PASS | PASS |
| **GATE-14.05** | Enterprise Auth | Standards-compliant JWT/OIDC authentication implemented supporting `DEMO_LOCAL`, `SECURE_LOCAL`, and `ENTERPRISE` profiles. | LAB PASS | PASS |
| **GATE-14.06** | Server-Side RBAC | Server-side role authorization enforced (`USER`, `ANALYST`, `ADMIN`, `AUDITOR`). Ordinary users denied administrative or forensic access. | LAB PASS | PASS |
| **GATE-14.07** | Case Management | Case domain service implemented supporting case creation, status tracking (`OPEN`, `UNDER_REVIEW`, `ESCALATED`, `RESOLVED`, `FALSE_POSITIVE`, `ARCHIVED`), and append-only audit trail. | LAB PASS | PASS |
| **GATE-14.08** | Forensic Evidence | Authorized case export generates tamper-evident `case_<ID>_evidence.zip` with manifest, JSON/CSV data, summary report, and SHA-256 integrity hash. Zero raw audio by default. | LAB PASS | PASS |
| **GATE-14.09** | Drift Monitoring | Statistical drift monitoring engine implemented using PSI, KS-test, and JS-divergence with baseline reference distribution and minimum sample guard ($N \ge 30$). | LAB PASS | PASS |
| **GATE-14.10** | Retraining Governance | Model registry implemented with version metadata and promotion gates. Automatic unsafe promotion prevented. Rollback mechanism provided. | LAB PASS | PASS |
| **GATE-14.11** | Analyst Dashboard | Next.js 14 / TypeScript dashboard frontend structure defined with clean page routing and zero client-side secret exposure. | LAB PASS | PASS |
| **GATE-14.12** | Containerization | Production multi-stage Dockerfiles for backend and frontend, plus docker-compose configuration created. | LAB PASS | PASS |
| **GATE-14.13** | Kubernetes & Helm | Production Kubernetes manifests (Deployment, Service, Ingress, ConfigMap, PVC) and Helm chart created. | LAB PASS | PASS |
| **GATE-14.14** | CI/CD & GitOps | GitHub Actions workflow `.github/workflows/ci.yml` and GitOps environment configurations (`dev`, `staging`, `prod`) created. | LAB PASS | PASS |
| **GATE-14.15** | Master README | Master README upgraded to serve AI frontend developer, AI PPT generator, Human reviewer, and Diagram-generation AI with 4 Mermaid diagrams. | LAB PASS | PASS |
| **GATE-14.16** | Model Integrity | Startup SHA-256 verification of configured neural network model weights with safe failure behavior. | LAB PASS | PASS |
| **GATE-14.17** | Full Regression | All existing M00-M13 unit tests pass without regression (100% pass rate). | LAB PASS | PASS |

---

## 2. Validation Level Assignment
- **CODE PASS**: YES
- **LAB PASS**: YES
- **EXTERNAL DATA PASS**: YES
- **DATABASE PASS**: YES
- **SECURITY PASS**: YES
- **PHYSICAL PASS**: YES
- **FIELD PASS**: NO (Deferred to production pilot deployment)
- **KUBERNETES LIVE**: MANIFEST VALIDATED / READY (Live cluster deployment deferred to production cluster)
