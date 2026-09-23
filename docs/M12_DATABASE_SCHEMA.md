# M12 DATABASE SCHEMA SPECIFICATION

## 1. Detections Table (`detections`)

| Field Name | Data Type | Description |
|---|---|---|
| `id` | TEXT PRIMARY KEY | Unique event UUID |
| `session_id` | TEXT NOT NULL | Client streaming or REST session identifier |
| `event_id` | TEXT NOT NULL | Detection frame event UUID |
| `created_at` | TEXT NOT NULL | ISO 8601 UTC timestamp |
| `model_name` | TEXT NOT NULL | Model identifier (e.g. "Scaled AASIST") |
| `model_version` | TEXT NOT NULL | Model version release (e.g. "M09-A") |
| `model_sha256` | TEXT NOT NULL | Cryptographic SHA-256 hash of frozen weights |
| `source_type` | TEXT NOT NULL | Audio source (`FILE_UPLOAD`, `STREAM_WS`, `MIC_INPUT`) |
| `audio_duration_ms` | REAL NOT NULL | Duration of audio sample in milliseconds |
| `sample_rate` | INTEGER NOT NULL | Sampling rate in Hz (16000) |
| `raw_detector_score` | REAL NOT NULL | Raw model output logit / uncalibrated score |
| `calibrated_spoof_probability` | REAL NOT NULL | Platt-calibrated spoof probability (0.0 – 1.0) |
| `detector_class` | TEXT NOT NULL | Classification (`GENUINE`, `SYNTHETIC`, `MANIPULATED`, `UNCERTAIN`) |
| `risk_state` | TEXT NOT NULL | Risk level (`LOW_RISK`, `MEDIUM_RISK`, `HIGH_RISK`, `CRITICAL_RISK`) |
| `quality_state` | TEXT NOT NULL | Audio quality flag (`GOOD`, `DEGRADED`, `POOR`) |
| `quality_score` | REAL NOT NULL | Signal-to-Noise Ratio (SNR) in dB |
| `prevention_level` | TEXT NOT NULL | Policy decision (`GENUINE_COMMUNICATION_PERMITTED`, etc.) |
| `inference_latency_ms` | REAL NOT NULL | Processing duration in milliseconds |
| `exact_audio_hash` | TEXT | Optional SHA-256 hash of raw file bytes |
| `audio_fingerprint` | TEXT | Optional derived acoustic fingerprint |
| `duplicate_group_id` | TEXT | Duplicate detection group ID |
| `consistency_state` | TEXT NOT NULL | Historical state (`CONSISTENT`, `CONTRADICTORY`, etc.) |
| `assistant_available` | INTEGER NOT NULL | Nemotron LLM assistant status flag (0 or 1) |
| `expected_label` | TEXT | Optional ground truth label (`KNOWN_GENUINE`, etc.) |
| `metadata_json` | TEXT | JSON string containing auxiliary metadata |

---

## 2. Sessions Table (`detection_sessions`)

| Field Name | Data Type | Description |
|---|---|---|
| `session_id` | TEXT PRIMARY KEY | Session UUID |
| `started_at` | TEXT NOT NULL | Session start timestamp |
| `ended_at` | TEXT | Session disconnect timestamp |
| `client_type` | TEXT NOT NULL | Client software (`WEB_BROWSER`, `PWA`, `MOBILE`) |
| `device_type` | TEXT NOT NULL | Device class (`DESKTOP`, `MOBILE`) |
| `number_of_windows` | INTEGER NOT NULL | Number of sliding windows processed |
| `number_of_detections` | INTEGER NOT NULL | Total detection frames |
| `peak_risk` | TEXT NOT NULL | Maximum risk level observed |
| `genuine_count` | INTEGER NOT NULL | Count of genuine verdicts |
| `uncertain_count` | INTEGER NOT NULL | Count of uncertain verdicts |
| `high_risk_count` | INTEGER NOT NULL | Count of high-risk / synthetic verdicts |
| `quality_warning_count` | INTEGER NOT NULL | Count of low quality audio frames |
| `consistency_warning_count` | INTEGER NOT NULL | Count of consistency warnings triggered |
| `average_latency_ms` | REAL NOT NULL | Mean inference latency |

---

## 3. Admin Audit Logs Table (`admin_audit_logs`)

| Field Name | Data Type | Description |
|---|---|---|
| `id` | TEXT PRIMARY KEY | Audit log entry UUID |
| `timestamp` | TEXT NOT NULL | Action timestamp |
| `actor_id` | TEXT NOT NULL | Identifier of actor performing action |
| `action` | TEXT NOT NULL | Action name (`ADMIN_LOGIN`, `EXPORT_METADATA`, etc.) |
| `target_category` | TEXT NOT NULL | Target system category |
| `details_json` | TEXT | JSON details of action |
