# Public vs. Private Repository Split (`docs/PUBLIC_PRIVATE_SPLIT.md`)

This document defines the explicit boundary between what is published to the public GitHub repository and what must remain in secure private archives or local developer machines.

---

## 🟢 Publicly Tracked in `vivikt-patra/VoxSatya`

These files and directories are intentionally public-facing to showcase the project for SIH 2026 evaluation and open-source credibility:

| Path | Reason for Public Inclusion |
|:---|:---|
| `README.md` | Primary showcase, team credits, architecture diagrams, and quickstart |
| `STRUCTURE.md` | Directory architecture for evaluators and contributors |
| `TESTING.md` | QA and benchmark instructions |
| `SECURITY_NOTES.md` | Privacy compliance and threat model disclosures |
| `CONTRIBUTING.md`, `CONTRIBUTORS.md` | Attribution and contribution guidelines |
| `checklists/` | Portfolio publishing gates (all are public-facing governance docs) |
| `docs/` | Milestone reports, architectural specs, screenshots |
| `backend/` | FastAPI source code (excluding compiled artifacts and `.env`) |
| `voxsatya-android/` | Kotlin source, test suites, AndroidManifest (excluding `.tooling/`, `build/`) |
| `voxsatya-mobile-web/` | React PWA source (excluding `node_modules/`, `dist/`) |
| `scripts/` | Automation, export, and deployment scripts |
| `.env.example` | Sanitized environment configuration template |
| `models/aasist/*.py`, `models/onnx/*.py` | PyTorch and ONNX architecture definitions |

---

## 🔴 Private Only — Never Commit to Public Repository

These items must remain exclusively in local development environments, internal team drives, or encrypted private archives:

| Asset Category | Reason for Private Restriction |
|:---|:---|
| **`.env`** | Contains live NVIDIA API keys, JWT secrets, database credentials |
| **`*.pt`, `*.pth`** (model weights) | Proprietary trained model weights representing months of GPU training |
| **`data/raw/`, `data/processed/`** | ASVspoof dataset audio — governed by academic licensing agreements |
| **`data/reference_voices/`** | Enrolled biometric voiceprints of actual human speakers |
| **`voice/`** | Recorded local test audio captured during lab evaluation |
| **`.tooling/`** | Android SDK and JDK platform-tools — large redistributable binaries |
| **`*.apk`, `*.aab`** | Compiled Android application binaries — distributed via GitHub Releases, not git |
| **`experiments/M14-a17/*.pt`** | Experimental weight variants from competitive hardening runs |
| **`data/runtime/*.db*`** | Live operational SQLite databases and WAL files from running instances |
| **`scratch/`** | Temporary developer debugging scripts and data files |
