# M14 FORENSIC EVIDENCE PACKAGING SPECIFICATION

## Executive Summary
Milestone 14 introduces tamper-evident forensic evidence export packages (`case_<CASE_ID>_evidence.zip`) for authorized analysts and administrators to export security cases for legal audit, compliance reporting, and incident investigation.

---

## Evidence Package Architecture

### ZIP Archive Structure
```text
case_case_a1b2c3d4e5f6_evidence.zip
├── case_summary.json         # Structured case metadata & risk evaluation
├── detections.json           # Array of linked audio detection outputs
├── detections.csv            # Tabular detection output CSV for spreadsheet import
├── timeline.json             # Chronological incident timeline
├── model_provenance.json     # Primary detector model name, version, and SHA-256
├── consistency_report.json   # Historical audio fingerprint consistency checks
├── content_risk_report.json  # Hinglish transcript risk & PII redaction audit
├── audit_log.json            # Append-only case audit trail
├── case_report.txt           # Human-readable printable summary report
├── README.txt                # Package documentation & usage rules
└── manifest.json             # Per-file SHA-256 checksums & export metadata
```

---

## Security & Integrity Invariants

1. **Tamper-Evident SHA-256 Manifest**:
   - `manifest.json` contains explicit SHA-256 checksums for every individual file inside the archive.
   - The export API returns `X-Evidence-Package-SHA256` in HTTP response headers.

2. **Zero Raw Audio Persistence**:
   - Audio PCM streams are NOT stored in the evidence archive by default to enforce privacy compliance.

3. **RBAC Authorization Enforcement**:
   - Only authenticated users with `ANALYST` or `ADMIN` roles can generate or download evidence packages. Attempts by ordinary users are logged and blocked with HTTP `403 Forbidden`.
