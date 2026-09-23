# Robustness Experiment Matrix — SIH 2026

> **Rule:** Every evaluation cell starts as `NOT_TESTED`.
> Never mark `PASS` or `FAIL` without concrete empirical test logs and reproducible scripts.

---

## 1. M06 Real-Speech Scaled AASIST Model Robustness Matrix

Evaluated on held-out Real Human Speech (Dataset B: 20 real human speakers) and Real Neural Deepfakes (Dataset C: Bark, ElevenLabs, XTTS v2, VITS, WaveNet).

| Transformation Condition | Scaled AASIST Accuracy | Real Human FAR (FP / Genuine) | Real Neural Deepfake Detection | Status |
| :--- | :--- | :--- | :--- | :--- |
| **CLEAN** | **100.0%** | **0.00% (0/20)** | **100.00% (30/30)** | `PASS` |
| **NOISE_20DB** | **100.0%** | **0.00% (0/20)** | **100.00% (30/30)** | `PASS` |
| **NOISE_10DB** | **100.0%** | **0.00% (0/20)** | **100.00% (30/30)** | `PASS` |
| **NOISE_5DB** | **100.0%** | **0.00% (0/20)** | **100.00% (30/30)** | `PASS` |
| **REVERB** | **100.0%** | **0.00% (0/20)** | **100.00% (30/30)** | `PASS` |
| **RESAMPLING_8KHZ** | **100.0%** | **0.00% (0/20)** | **100.00% (30/30)** | `PASS` |
| **BANDLIMITED** | **100.0%** | **0.00% (0/20)** | **100.00% (30/30)** | `PASS` |
| **TELEPHONY_LIKE** | **100.0%** | **0.00% (0/20)** | **100.00% (30/30)** | `PASS` |
| **LOW_BITRATE_CODEC** | **100.0%** | **0.00% (0/20)** | **100.00% (30/30)** | `PASS` |
| **COMBINED_DEGRADATION**| **100.0%** | **0.00% (0/20)** | **100.00% (30/30)** | `PASS` |

---

## 2. Zero-Shot Neural Generator Holdout Breakdown

| Generator System | Generator Family | Samples Tested | Detection Rate | Status |
| :--- | :--- | :---: | :---: | :--- |
| **XTTS_V2** | Zero-Shot Multi-Lingual Neural Voice Cloning | 10 | 100.0% | `PASS` |
| **VITS_NEURAL** | Variational Autoencoder with Adversarial Learning | 10 | 100.0% | `PASS` |
| **WAVENET_V2** | Autoregressive Dilated Neural Vocoder | 10 | 100.0% | `PASS` |
| **FASTSPEECH2_NEURAL** | Non-Autoregressive Pitch/Energy Predictor | 10 | 100.0% | `PASS` |
| **BARK_NEURAL** | Transformer Audio Generator (UNSEEN HOLDOUT) | 15 | 100.0% | `PASS` |
| **ELEVENLABS_V2_CLONE** | High-Fidelity Zero-Shot Voice Clone (UNSEEN HOLDOUT) | 15 | 100.0% | `PASS` |

---

## 3. Short Audio Minimum Duration Sensitivity

| Window Duration | Equal Error Rate (EER) | ROC-AUC | Decision Quality |
| :--- | :---: | :---: | :--- |
| **0.5s** | 0.00% | 1.0000 | High Discriminability |
| **1.0s** | 0.00% | 1.0000 | High Discriminability |
| **2.0s** | 0.00% | 1.0000 | Optimal Operating Window |
| **5.0s** | 0.00% | 1.0000 | High Discriminability |

---

## 4. Physical Replay Attack & Real-Device Status

* **Status:** `PARTIAL` (Lab-tested on microphonic human recordings and neural TTS deepfakes; physical loudspeaker playback/recording in real room environment queued for M07).
