# M12 ADMIN ACCESS & EXPORT SPECIFICATION

## 1. Role-Based Access Control Model

| Role | Access Level | Permitted Operations | Restricted Operations |
|---|---|---|---|
| **USER** (Ordinary User / Client App) | Low | Stream live audio over WebSocket, submit single files to `/api/v1/audio/analyze`, receive current risk verdict & prevention policy | CANNOT query `/api/v1/admin/*`, CANNOT view historical database records, CANNOT export metadata, CANNOT access database files |
| **ADMIN** (Authorized Operator) | High | View tabular detection history, filter by class/risk/consistency, view single detection detail & model provenance, view session stats, export JSON/CSV metadata | Subject to server-side token authentication & audit logging |

---

## 2. Protected Admin API Endpoints

- `POST /api/v1/admin/login`: Admin password authentication -> returns Bearer token.
- `GET /api/v1/admin/detections`: Paginated list of detection records (Server-side admin token required).
- `GET /api/v1/admin/detections/{id}`: Detailed record view including model SHA256 and consistency report.
- `GET /api/v1/admin/sessions`: Paginated session summaries.
- `GET /api/v1/admin/consistency-warnings`: List of contradictory or cross-model detection events.
- `GET /api/v1/admin/stats`: Aggregate system statistics.
- `POST /api/v1/admin/export`: Export detection metadata as JSON or CSV (`m12_detection_export.json`/`.csv`).
