# M07 Frozen-Model External Speech Benchmark Report

## Executive Summary
This document records the **frozen-model reality check** performed on the Scaled AASIST anti-spoof detector against the independently acquired ASVspoof 2019 Logical Access external dataset. Absolutely **zero model retraining, fine-tuning, or threshold adjustment** was performed prior to evaluation.

## 1. Safety & Freeze Verification
- **Model Architecture**: Scaled AASIST (`AASISTModel`)
- **Checkpoint Path**: `models/scaled/aasist/best_model.pt`
- **Calibrator Temperature**: $T = 0.3241$ (`models/scaled/aasist/calibrator.json`)
- **Decision Threshold**: $0.50$ (Fixed)
- **Retraining Status**: **ZERO RETRAINING / 100% FROZEN**

## 2. External Baseline Performance Metrics
| Metric | Frozen Model Value | Notes / Description |
| :--- | :---: | :--- |
| **Accuracy** | **54.73%** | Overall classification accuracy at 0.50 threshold |
| **Equal Error Rate (EER)** | **42.31%** | Operating point where FPR equals FNR |
| **ROC-AUC** | **0.5773** | Area under ROC curve |
| **Precision** | **1.0000** | True Synthetic / Predicted Synthetic |
| **Recall** | **0.0891** | True Synthetic Detected / Total Synthetic |
| **F1 Score** | **0.1636** | Harmonic mean of Precision and Recall |
| **False Positive Rate (FPR)** | **0.00%** | Genuine human speech falsely classified as synthetic |
| **False Negative Rate (FNR)** | **91.09%** | Neural spoof speech falsely classified as genuine |

## 3. Raw Count Breakdown
- **Total Acquired Samples (N)**: 497
- **Genuine Human Samples**: 250
- **Neural Spoof Samples**: 247
- **False Positives (FP)**: **0 / 250** (Genuine classified as Synthetic)
- **False Negatives (FN)**: **225 / 247** (Spoof classified as Genuine)
- **Uncertain Classifications**: 1 / 497 (0.2%)

## 4. Short-Audio External Evaluation
Evaluated model on truncated actual audio segments across duration bounds:
| Segment Duration | Sample Count (N) | Accuracy | Equal Error Rate (EER) |
| :--- | :---: | :---: | :---: |
| **0.5 sec** | 497 | 50.30% | 47.37% |
| **1.0 sec** | 497 | 50.30% | 47.77% |
| **2.0 sec** | 497 | 54.73% | 42.31% |
| **5.0 sec** | 497 | 54.73% | 42.31% |
| **Full Utterance** | 497 | 54.73% | 42.31% |

## 5. Negative & Sanity Control Experiments
- **Label-Shuffle Control Accuracy**: **49.09%** (Consistent with ~50% random guessing level, proving evaluation pipeline integrity).
- **Label-Shuffle ROC-AUC**: **0.4916**

## 6. Official Validation Ladder Milestone Status
- [x] **CODE PASS**: Fully implemented pipeline & pass 78+ unit tests.
- [x] **LAB PASS**: Passed local sanity and cue normalization experiments.
- [x] **EXTERNAL DATA PASS**: Authenticated and benchmarked on real external ASVspoof 2019 LA dataset.
- [ ] **PHYSICAL PASS**: Deferred until physical acoustic playback/recording tests.
- [ ] **FIELD PASS**: Deferred until real-world user field deployment.