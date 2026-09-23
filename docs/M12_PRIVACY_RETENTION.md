# M12 PRIVACY & RETENTION POLICY SPECIFICATION

## 1. Privacy-by-Default (Zero Raw Audio Persistence)
The system strictly enforces `RAW_AUDIO_STORAGE = OFF` across all production endpoints:
- No raw microphone PCM bytes or audio files are persisted to disk or database.
- Streaming rolling audio buffers are held strictly in temporary RAM and cleared immediately when WebSocket sessions close (`session.clear()`).
- Only non-reversible derived evidence (cryptographic SHA-256 file hashes, quality metrics, calibrated probabilities, risk verdicts) is stored.

---

## 2. Retention Policy
- Detection metadata records are stored in `data/runtime/detections.db`.
- Audit logs retain administrative login and export events.
- Production deployments can configure metadata retention limits via environment variable `METADATA_RETENTION_DAYS`.
