# M14 CASE MANAGEMENT ARCHITECTURE & AUDIT SPECIFICATION

## Executive Summary
Milestone 14 introduces the **Case** abstraction, transitioning the system from individual audio frame detection rows into structured security incident investigation workflows.

---

## Case Lifecycle & Schema

### Case Status Progression
```text
  ┌──────┐     ┌──────────────┐     ┌───────────┐     ┌──────────┐
  │ OPEN │ --> │ UNDER_REVIEW │ --> │ ESCALATED │ --> │ RESOLVED │
  └──────┘     └──────────────┘     └───────────┘     └──────────┘
                      │                                    │
                      ▼                                    ▼
              ┌────────────────┐                    ┌──────────┐
              │ FALSE_POSITIVE │                    │ ARCHIVED │
              └────────────────┘                    └──────────┘
```

### Case Schema Fields
- `case_id`: Unique identifier (e.g. `case_a1b2c3d4e5f6`).
- `created_at` / `updated_at`: ISO-8601 timestamps.
- `status`: Enum (`OPEN`, `UNDER_REVIEW`, `ESCALATED`, `RESOLVED`, `FALSE_POSITIVE`, `ARCHIVED`).
- `severity`: Enum (`LOW`, `MEDIUM`, `HIGH`, `CRITICAL`).
- `assigned_analyst`: Username of assigned analyst.
- `title` & `summary`: Descriptive case title and security investigation notes.
- `voice_risk`, `content_risk`, `combined_risk`: Evaluated risk states.
- `detection_ids`: List of linked detection records from SQLite database.
- `evidence_count`: Number of linked detection samples.
- `model_versions`: Provenance tracking of models used.
- `notes`: Append-only list of analyst notes.

---

## Append-Only Audit Trail Invariant
Every action taken on a case (creation, assignment, status update, note addition, evidence export) generates an immutable, append-only record in the `case_audit_logs` database table. Historical security evidence is NEVER overwritten or silently deleted.
