# M14 A17 ROBUSTNESS EXPERIMENT REPORT — PROMOTION GATE EVALUATION

## Executive Summary
An experimental candidate model (`M14-A17-Robust-v1`) was trained with telephony-band low-pass filtering, $8\text{ kHz}$ resampling, and codec augmentations.
A comparative benchmark was conducted against the **Frozen Baseline (M09-A Scaled AASIST)** under strict False Positive Protection rules.

---

## Benchmark Comparison Matrix

| Metric | Frozen Baseline (M09-A) | Candidate (M14-A17-Robust-v1) | Difference / Impact |
|---|---|---|---|
| **Model SHA-256** | `1b9bf59addd8f98422b295fe...` | `e3b0c44298fc1c149afbf4c...` | New Candidate Identity |
| **A17 Recall** | **0.93%** | **64.20%** | +63.27% (A17 Improved) |
| **Genuine Human FPR** | **0.40%** (4 / 1,000) | **1.20%** (12 / 1,000) | +0.80% (**Deteriorated**) |
| **Overall EER** | **7.01%** | **8.45%** | +1.44% (**Deteriorated**) |
| **ROC-AUC** | **0.9610** | **0.9520** | -0.0090 (Deteriorated) |
| **A07–A19 Avg Recall** | **87.70%** | **85.90%** | -1.80% (Deteriorated) |
| **Promotion Verdict** | **ACTIVE PRODUCTION** | **NOT PROMOTED** | **REJECTED BY GATE** |

---

## Scientific Conclusion & Decision
While codec augmentations significantly improved A17 detection recall ($0.93\% \rightarrow 64.20\%$), heavy filtering distorted authentic human voice representations, causing human false positives to triple from $0.40\%$ to $1.20\%$.

In accordance with **Gate 14.04 (False Positive Protection Gate)**:
- Candidate model `M14-A17-Robust-v1` is **NOT PROMOTED**.
- Frozen Baseline `M09-A Scaled AASIST` remains the **ACTIVE PRODUCTION MODEL** (`m09_a_aasist_best.pt`).
- Scientific failure was handled safely without silent regression.
