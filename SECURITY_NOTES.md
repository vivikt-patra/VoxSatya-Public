# VoxSatya Security, Privacy & Compliance Notes (`SECURITY_NOTES.md`)

This document outlines the security architecture, data governance policies, threat vectors, and statutory compliance framework enforced across the **VoxSatya** platform.

---

## 🛡️ 1. Threat Model & Defense Vectors

VoxSatya is designed to neutralize weaponized generative speech synthesis within the Indian telecommunications ecosystem:

```text
┌─────────────────────────────────────────────────────────────┐
│                 ADVERSARY ATTACK VECTORS                    │
├──────────────────────────────┬──────────────────────────────┤
│ 1. "Digital Arrest" Scams    │ Impersonation of CBI, ED, or │
│                              │ Police officials via voice   │
│ 2. Fake Kidnapping Hoaxes    │ Scraped voice clones of kin  │
│ 3. USSD Forwarding Exploits  │ *401* MMI call-interception  │
│ 4. OTP / PIN Solicitation    │ High-pressure financial loss │
└──────────────────────────────┴──────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                 VOXSATYA DEFENSE INVARIANTS                 │
├─────────────────────────────────────────────────────────────┤
│ • Invariant 1: Audio is Sole Authority for Voice Authenticity│
│ • Invariant 2: Zero Raw Telephony Audio Disk Retention      │
│ • Invariant 3: Zero Network Egress During Live Call Defense │
│ • Invariant 4: Cryptographic Immutability of Forensic Records│
└─────────────────────────────────────────────────────────────┘
```

---

## 📜 2. Statutory & Regulatory Compliance

### A. Digital Personal Data Protection (DPDP) Act 2023
- **Data Minimization**: VoxSatya processes only volatile PCM audio frames in transient RAM ring buffers. Once neural inference is complete, raw audio buffers are immediately overwritten and discarded.
- **Purpose Limitation**: Audio signals are analyzed solely for synthetic spoofing detection and vocal duress biometrics; no personal identifiers are extracted or retained without explicit user consent.
- **Zero Cloud Wiretapping**: Live cellular phone calls are intercepted and evaluated 100% on the local handset via Android HAL `AudioRecord` and ONNX Runtime Mobile, eliminating third-party cloud wiretapping liabilities under the Indian Telegraph Act.

### B. Indian Evidence Act Section 65B (Electronic Evidence Admissibility)
- To ensure forensic records withstand legal scrutiny in Indian courts and judicial proceedings, VoxSatya implements an on-device **SHA-256 Chained Forensic Ledger** (micro-blockchain in Room SQLite).
- Every detection record stores:
  - Epoch timestamp and call duration.
  - Model version and calibrated spoof probability.
  - DSP vocal micro-tremor metrics (Jitter %, Shimmer dB).
  - Previous Block SHA-256 Hash and Current Record SHA-256 Hash.
- Any attempt to alter historical records breaks the hash chain, immediately exposing tampering.

### C. Citizen Financial Cyber Fraud Reporting System (CFCFRMS / 1930)
- VoxSatya provides a one-tap export mechanism that generates structured, standardized incident reports formatted for direct integration with the **National Cyber Crime Reporting Portal (1930)** and **CERT-In**.
- Reports are exported via the Android 14 `MediaStore.Downloads` Scoped Storage API without requiring legacy `WRITE_EXTERNAL_STORAGE` permissions.

---

## 🔑 3. Credential & Secret Governance

- **Zero Hardcoded Secrets**: No API keys, database passwords, or private encryption keys may be committed to this repository.
- **Environment Isolation**: Local secrets must reside exclusively in a `.env` file that is permanently ignored by `.gitignore`.
- **Sanitized Templates**: Only `.env.example` containing empty placeholder keys is tracked in version control.
- **Automated Scanning**: The pre-push audit script (`scripts/test_track_b_nemotron.py`) automatically scans every tracked file for patterns resembling API keys or tokens before release.

---

## 🚨 4. Vulnerability Disclosure Policy

If you discover a security vulnerability or potential threat bypass in VoxSatya, please do not file a public GitHub issue. Instead, report it privately to the security team:

- **Contact**: Team Lead via GitHub Security Advisory or direct repository maintainer communication.
- **Response SLA**: Initial triage within 48 hours; remediation patch within 7 days.
