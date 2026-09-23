# M12 CONSISTENCY INTELLIGENCE & ADVISORY WARNING ENGINE

## 1. Executive Overview
The **Consistency Intelligence Engine** ([`ml/persistence/consistency.py`](file:///c:/Users/vivik/OneDrive/Documents/Apex_Coding/SIH_2026_PS/ml/persistence/consistency.py)) evaluates historical detection records for the exact same or similar controlled audio inputs to detect contradictory results across multiple runs or model versions.

---

## 2. Consistency States

1. **`CONSISTENT`**: The current detection verdict matches all historical evaluations for this audio SHA-256 hash.
2. **`MINOR_VARIATION`**: Verdict matches, but probability score varies slightly ($\Delta > 0.15$).
3. **`CONTRADICTORY`**: The **same model version** previously produced a conflicting classification (e.g. previously `GENUINE`, now `SYNTHETIC`).
4. **`CROSS_MODEL_VARIATION`**: A **different model version** or model SHA256 produced a different classification (e.g. `M06-CNN` output `GENUINE`, while `M09-A` output `SYNTHETIC`).
5. **`INSUFFICIENT_HISTORY`**: Fewer than 2 historical runs exist for this audio hash.

---

## 3. Strict Non-Overwrite Rule (Advisory Safety Invariant)

> **CRITICAL INVARIANT**:  
> Historical consistency is **strictly advisory**. The `ConsistencyAnalyzer` never mutates or overwrites the current detector score, probability, or classification verdict.
>
> If a sample previously evaluated as `HIGH_RISK` is currently evaluated by the detector as `GENUINE`, the system reports:
> - **Detector Classification**: `GENUINE`
> - **Consistency State**: `CONTRADICTORY`
> - **Advisory Warning**: "Warning: Same model previously classified this exact audio as ['SYNTHETIC']."
