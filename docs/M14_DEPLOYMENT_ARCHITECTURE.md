# M14 PRODUCTION DEPLOYMENT & CONTAINERIZATION SPECIFICATION

## Executive Summary
Milestone 14 introduces production-ready containerization, Kubernetes manifest configurations, Helm charts, and CI/CD pipelines.

---

## Deployment Artifacts

1. **Docker Container Images**:
   - `deploy/docker/Dockerfile.backend`: Multi-stage Python 3.11 build for FastAPI API server.
   - `deploy/docker/Dockerfile.frontend`: Multi-stage Node.js 18 build for Next.js analyst dashboard.
   - `deploy/docker/docker-compose.yml`: Local integration environment linking backend and frontend containers.

2. **Kubernetes Manifests (`deploy/k8s/`)**:
   - `deployment.yaml`: Configures 2 backend pod replicas with Liveness (`/health/liveness`) and Readiness (`/health/readiness`) probes.
   - `values.yaml` & `Chart.yaml`: Configurable Helm chart (`deploy/helm/sih-voice-defense/`).

3. **CI/CD Pipeline (`.github/workflows/ci.yml`)**:
   - Executes Python unit test suite on pull requests and pushes.
   - Verifies model artifact SHA-256 integrity (`ml/models/verifier.py`).
   - Builds Docker container images for release tags.

4. **GitOps Readiness**:
   - Declarative manifest structure supporting Kustomize & Helm GitOps workflows (`GITOPS-READY`).
