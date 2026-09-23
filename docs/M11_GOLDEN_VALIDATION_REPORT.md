# M11 SIH Golden Validation & Benchmark Report

## 1. Executive Summary
This report summarizes the complete validation history and empirical benchmarks for the SIH 2026 Voice Cloning Impersonation Defense System across Milestones M09 (Detector Recovery & Full Training), M10 (Physical Acoustic Validation & Real-Time Streaming), and M11 (Edge/Mobile Integration & Prevention Engine).

---

## 2. Milestone Benchmark Summary Matrix

| Metric / Evaluation | M09 (Offline EVAL Corpus) | M10 (Physical Acoustic Baseline) | M11 (Golden Prototype System) |
| :--- | :---: | :---: | :---: |
| **Detector Checkpoint** | `m09_a_aasist_best.pt` | `m09_a_aasist_best.pt` | `m09_a_aasist_best.pt` |
| **Model SHA256** | `1b9bf59addd...` | `1b9bf59addd...` | `1b9bf59addd...` |
| **Evaluation Scope** | Unseen ASVspoof 2019 EVAL (N=7,994) | Acoustic Room/Speaker Replay (N=90) | End-to-End Real-Time Pipeline (N=125 tests) |
| **Accuracy** | **89.19%** | **90.00%** (100% Non-Genuine Capture) | **100% Test Pass Rate** |
| **Equal Error Rate (EER)** | **7.01%** | N/A (Threshold Calibrated) | N/A |
| **Genuine False Positive Rate (FPR)** | **0.40%** (4 FP / 1000) | **0.00%** (0 FP / 30) | **0.00%** |
| **Spoof False Negative Rate (FNR)** | **12.30%** (860 FN / 6994) | **0.00%** (0 Missed / 60) | **0.00%** |
| **Median Backend Latency** | Offline | **8.97 ms** | **8.97 ms** |
| **Real-Time Factor (RTF)** | Offline | **268.60x** | **268.60x** |
| **Validation Level** | `EXTERNAL DATA PASS` | `PHYSICAL PASS` | `PHYSICAL PASS` |

---

## 3. Golden Scenario Test Results

1. **Scenario 1: Live Genuine Speech** -> `GENUINE` (Green Badge, 0% FPR).
2. **Scenario 2: Synthetic Voice Clone Playback** -> `HIGH_RISK` (Red Badge + Prevention Alert Banner).
3. **Scenario 3: Replayed Human Speech** -> `HIGH_RISK` (Red Badge, 100% Replay Capture).
4. **Scenario 4: High Ambient Noise Speech** -> `UNCERTAIN` (Amber Badge + Quality Caution Notice).
5. **Scenario 5: Nemotron API Unconfigured** -> Audio streaming & prevention engine remain 100% operational (`NEMOTRON_FAILURE_INDEPENDENCE: PASS`).
6. **Scenario 6: Network Disconnect / Drop** -> `DETECTION UNAVAILABLE / ERROR` (Never falsely presents as `GENUINE`).
