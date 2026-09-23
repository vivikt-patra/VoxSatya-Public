# Baseline Error Analysis — Milestone 02

> **Notice:** This document analyzes False Positives (bonafide human speech misclassified as spoof) and False Negatives (synthetic/cloned audio misclassified as bonafide) across Baseline A (Classical Logistic Regression) and Baseline B (Small PyTorch CNN) on **unseen zero-shot generator attacks**.

---

## 1. Zero-Shot Attack Benchmark Setup

To prevent data leakage and evaluate generalization against unseen voice cloning algorithms:
* **Train Split:** Generators `GEN_01` (Tacotron 2 simulation), `GEN_02` (WaveNet simulation), `GEN_03` (Voice Conversion simulation) across Speakers `SPK_01`–`SPK_10`.
* **Validation Split:** Generators `GEN_01`, `GEN_02`, `GEN_03` across Speakers `SPK_11`–`SPK_14`.
* **Held-Out Test Split:** **UNSEEN GENERATORS** `GEN_04` (VALL-E/ElevenLabs pitch jitter simulation) & `GEN_05` (Diffusion/Tortoise stochastic noise simulation) across Speakers `SPK_15`–`SPK_18`.

---

## 2. Zero-Shot Held-Out Evaluation Summary

| Baseline Model | Feature Representation | EER (%) | FAR / FNR (%) | FRR / FPR (%) | Accuracy (%) | F1-Score | Mean Total Latency |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **Baseline A (Classical)** | MFCC + Stats (80-dim) | 0.00% | 15.00% | **0.00%** | 92.50% | 0.9189 | 0.908 ms |
| **Baseline B (PyTorch CNN)**| Log-Mel Spectrogram | **0.00%** | **0.00%** | **0.00%** | **100.00%** | **1.0000** | 2.157 ms |

### Confusion Matrix Breakdown (40 Held-Out Zero-Shot Test Samples):
* **Baseline A (Classical MFCC + LogReg):**
  * True Negatives (TN - Genuine Correct): 20
  * **False Positives (FP - Genuine Wrongly Flagged): 0 (0.00% False Positive Rate)**
  * False Negatives (FN - Unseen Spoof Misclassified as Genuine): 3 (15.0% FNR)
  * True Positives (TP - Spoof Correct): 17
* **Baseline B (Small PyTorch CNN + Log-Mel Spectrogram):**
  * True Negatives (TN - Genuine Correct): 20
  * **False Positives (FP - Genuine Wrongly Flagged): 0 (0.00% False Positive Rate)**
  * **False Negatives (FN - Unseen Spoof Misclassified as Genuine): 0 (0.00% False Negative Rate)**
  * True Positives (TP - Spoof Correct): 20

---

## 3. Key Findings

1. **Zero False Positive Rate (FRR = 0.00%):** Both Baseline A and Baseline B achieved 0 False Positives on genuine human speech. No natural human speaker was misclassified as spoof.
2. **Neural Feature Superiority on Zero-Shot Generators:** Baseline A (Classical MFCC summary stats) failed on 3 unseen synthetic generator samples (FN = 3) because summary statistics averaged out subtle pitch jitter artifacts. Baseline B (PyTorch CNN operating on 2D Log-Mel Spectrograms) detected 100% of unseen generator attacks (FN = 0) with zero performance degradation.
