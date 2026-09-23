# M14 STATISTICAL MODEL & DATA DRIFT MONITORING SPECIFICATION

## Executive Summary
Milestone 14 introduces a real statistical drift-monitoring engine to track changes in production audio feature distributions, detector confidence scores, and noise levels over time.

---

## Statistical Measures & Thresholds

1. **Population Stability Index (PSI)**:
   $$\text{PSI} = \sum_{i=1}^{B} (P_i - Q_i) \times \ln\left(\frac{P_i}{Q_i}\right)$$
   - $\text{PSI} < 0.10$: **`STABLE`** (Production distribution matches baseline).
   - $0.10 \le \text{PSI} < 0.25$: **`WATCH`** (Mild distribution shift observed).
   - $\text{PSI} \ge 0.25$: **`DRIFT_DETECTED`** (Significant distribution shift).

2. **Minimum Sample Size Guard ($N \ge 30$)**:
   - Batches with fewer than $30$ audio samples automatically return `INSUFFICIENT_DATA` to prevent false drift alerts from noisy small samples.

3. **Strict Separation of Data Drift vs Performance Drift**:
   - **`DATA DRIFT`**: Detects changes in input distributions (e.g. new microphone codecs, higher noise floors, different language mix).
   - **`PERFORMANCE DRIFT`**: Measures degradation in EER or Accuracy. Requires ground-truth labels. When ground truth is unavailable in live traffic, the engine explicitly reports:
     `DATA DRIFT OBSERVED, PERFORMANCE IMPACT UNKNOWN (Unlabeled Production Traffic)`.
