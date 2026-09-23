# M10 Frozen M09 Physical Baseline

## 1. Baseline Verification Summary
- **Historical Commit**: `85d2254`
- **Checkpoint Tag**: `checkpoint/M09`
- **Model Checkpoint Artifact**: `m09_a_aasist_best.pt`
- **Verified SHA256**: `1b9bf59addd8f98422b295fe7ab613b48b0e93eaaadceb0d8ffb5a81d53bda39`
- **Verification Status**: `M09_BASELINE_VERIFIED = YES`

> [!IMPORTANT]
> The M09 detector weights (`m09_a_aasist_best.pt`) remained strictly frozen during all M10 real-time and physical acoustic evaluations. No retraining or weight modifications were performed.

## 2. Frozen Physical Evaluation Results

| Physical Attack Category | Sample Size (N) | Correctly Identified | Uncertain | False High-Risk / Missed | Rate (%) |
| :--- | :---: | :---: | :---: | :---: | :---: |
| **LIVE_HUMAN** (Genuine Speech) | 30 | 29 | 1 | 0 (False High-Risk) | **FPR: 0.00%** |
| **REPLAYED_HUMAN** (Acoustic Replay) | 30 | 30 | 0 | 0 (Missed) | **Detection Rate: 100.00%** |
| **SYNTHETIC_PLAYBACK** (Voice Clone) | 30 | 24 | 6 | 0 (Missed) | **Detection Rate: 80.00%** (100% Non-Genuine) |

## 3. Real-Time Latency & Performance

| Latency Metric | Measured Value |
| :--- | :--- |
| Preprocessing Median Latency | 0.12 ms |
| Model Inference Median Latency | 5.73 ms |
| Model Inference P95 Latency | 7.00 ms |
| End-to-End Decision Median Latency | **8.97 ms** |
| End-to-End Decision P95 Latency | **10.64 ms** |
| Real-Time Factor (RTF) | **268.60x** |

## 4. Key Findings & Baseline Status
1. **False Positive Safety**: Frozen M09 AASIST achieved 0.00% false positive rate on genuine human speech under quiet room conditions.
2. **Replay Sensitivity**: Replayed human audio through physical speaker and room acoustics was detected with 100.00% accuracy.
3. **Synthetic Speech Handling**: Synthetic voice clone playback was captured with 80.00% high-risk classification and 20.00% uncertain classification, with 0% genuine false negatives.
4. **Latency Excellence**: End-to-end processing latency of 8.97ms per 4.0-second window is well below the target 100ms real-time latency threshold.
