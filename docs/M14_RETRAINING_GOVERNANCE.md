# M14 MODEL RETRAINING GOVERNANCE & REGISTRY SPECIFICATION

## Executive Summary
Milestone 14 establishes a controlled, reproducible retraining framework. Automatic replacement of production models upon data drift detection is **strictly prohibited**.

---

## Controlled Retraining Workflow
```text
  ┌──────┐     ┌──────────────────┐     ┌─────────────────┐     ┌────────────────┐
  │ DRIFT│ --> │ CANDIDATE DATASET│ --> │ RETRAINING JOB  │ --> │ CANDIDATE MODEL│
  └──────┘     └──────────────────┘     └─────────────────┘     └────────────────┘
                                                                        │
                                                                        ▼
  ┌──────────┐     ┌─────────────────┐     ┌────────────────┐     ┌──────────────┐
  │PROMOTED /│ <-- │  ADMIN APPROVAL │ <-- │ PROMOTION GATE │ <-- │ EVAL BENCHMARK│
  │ REJECTED │     └─────────────────┘     └────────────────┘     └──────────────┘
  └──────────┘
```

---

## Model Status Lifecycle

- **`CANDIDATE`**: Model produced by a retraining job awaiting benchmark evaluation.
- **`VALIDATED`**: Model passed benchmark metrics but pending administrative sign-off.
- **`PROMOTED`**: Active production model serving inference requests.
- **`REJECTED`**: Candidate model failed promotion gates (e.g. higher human false positives).
- **`ARCHIVED`**: Previously promoted model demoted via rollback.

---

## Promotion & Rollback Invariants

1. **Human False Positive Protection Gate**:
   - A candidate model is REJECTED if its False Positive Rate (FPR) on authentic human speech exceeds the baseline threshold ($FPR > 0.40\%$).

2. **Rollback Manager**:
   - Allows administrators to demote an active production model and restore a previous validated model pointer instantaneously without data loss.
