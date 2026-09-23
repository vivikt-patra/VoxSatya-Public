# M09 Pre-EVAL Freeze Manifest

Generated: 2026-09-06 05:01:40 UTC
Git Commit: 2a8331b94e637225f4e1fcb72352288f4e99feae

## 1. Selected Primary Detector: M09-A Scaled AASIST
- Checkpoint Path: experiments\M09-detector-recovery\m09_a_aasist_best.pt
- Checkpoint SHA256: `1b9bf59addd8f98422b295fe7ab613b48b0e93eaaadceb0d8ffb5a81d53bda39`
- Architecture: Scaled AASIST (SincConv + Graph Attention + MaxFeatureMap)
- Total Parameters: 628,082
- Checkpoint File Size: 7.61 MB

## 2. DEV Selection Metrics (DEV Only)
- Evaluation Basis: Full ASVspoof 2019 LA DEV Split (24,844 samples)
- DEV EER: 0.06%
- DEV ROC-AUC: 1.0000
- DEV FPR: 0.43% (FP count: 11/2,548)
- DEV FNR: 0.00% (FN count: 1/22,296)
- DEV F1-Score: 0.9997

## 3. Calibration & Decision Policy (DEV Only)
- Method: Platt Scaling
- Parameters: a = 1.038813, b = -2.605226
- Frozen Decision Thresholds:
  - GENUINE   : prob < 0.0033
  - UNCERTAIN : [0.0033, 0.9973)
  - SYNTHETIC : prob >= 0.9973
  - Operating EER Threshold: 0.8891

## 4. Hard Freeze Declaration
ALL MODEL WEIGHTS, FEATURE EXTRACTORS, HYPERPARAMETERS, CALIBRATION PARAMETERS, AND DECISION THRESHOLDS ARE OFFICIALLY FROZEN.
No parameter modifications or tuning will take place after unsealing the EVAL split.

Signed: Antigravity AI Pair Programmer
Timestamp: 2026-09-06 05:01:40 UTC
