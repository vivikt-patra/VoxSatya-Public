# M14 CYBERSECURITY THREAT MODEL & AUDIT REPORT

## Comprehensive Threat Model

| Asset | Threat | Security Control | Residual Risk |
|---|---|---|---|
| **Model Weights** | Weight tampering / Unauthorized replacement | Startup SHA-256 integrity verification (`ml/models/verifier.py`). | LOW |
| **API Endpoints** | Role escalation / Unauthorized admin access | Server-side RBAC dependencies (`require_roles`). | LOW |
| **Case Evidence** | Evidence package exfiltration / Tampering | Per-file SHA-256 manifest and package checksums. RBAC restricted. | LOW |
| **User Privacy** | Audio PCM leak / Financial PII storage | Zero-Disk Audio Persistence Policy & PII Scrubbing (`ml/asr/normalizer.py`). | LOW |
| **Retraining Data** | Data poisoning attack | Strict dataset manifest provenance check & mandatory admin approval. | LOW |
| **Hinglish ASR** | Prompt injection via transcript | Authority boundary firewall: Audio AASIST is sole authority for voice clone classification. | LOW |

---

## Model Artifact Integrity Verification
The backend automatically computes the SHA-256 checksum of `models/m09_a_aasist_best.pt` during readiness probes:
- Expected Hash: `1b9bf59addd8f98422b295fe7ab613b48b0e93eaaadceb0d8ffb5a81d53bda39`
- Action on Mismatch: Backend readiness probe returns HTTP `503 Service Unavailable`, preventing untrusted weights from serving live traffic.
