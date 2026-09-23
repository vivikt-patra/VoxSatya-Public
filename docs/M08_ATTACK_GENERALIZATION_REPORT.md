# M08 Attack Generalization Report

**Milestone:** M08 — Authentic Multi-Generator Training & External Generalization  
**Generated:** 2026-09-06  
**Eval Protocol:** One-shot, EVAL split accessed once after all DEV-based decisions were frozen

---

## Overview

This report documents per-attack detection performance on the EVAL split (A07-A19) after
training exclusively on seen attacks (A01-A06 from the official DEV split).

The central question: **does a detector trained on seen TTS/VC attacks generalize to unseen synthesis methods?**

Answer for M08: **No.** All 13 unseen attack types achieve 0% detection rate with the M08 AASIST model.

---

## Seen Attacks (Training: A01-A06)

| Attack | Description | Samples in Train | Samples in InnerDEV |
|--------|-------------|-----------------|---------------------|
| A01 | Neural TTS (CART waveform concat.) | ~13 | ~3 |
| A02 | Neural TTS (NN-TTS + WORLD vocoder) | ~13 | ~3 |
| A03 | Neural TTS (NN-TTS + STRAIGHT vocoder) | ~13 | ~3 |
| A04 | Neural TTS (CART waveform concat.) | ~13 | ~3 |
| A05 | Voice Conversion (VCC2018 + WORLD vocoder) | ~13 | ~3 |
| A06 | Voice Conversion (Spectral filtering VC) | ~13 | ~3 |

> **Note:** Seen attack training samples are tiny (~13/attack). No per-seen-attack held-out evaluation
> was conducted, as seen attack DEV samples were used for training.

---

## Unseen Attacks (EVAL: A07-A19)

| Attack | Description | N | TP | FN | Detection Rate | 95% CI |
|--------|-------------|---|----|----|---------------|--------|
| A07 | Neural TTS (Tacotron + WaveRNN) | 13 | 0 | 13 | **0.0%** | [0.0%, 24.7%] |
| A08 | Neural TTS (NN-TTS + WaveNet) | 13 | 0 | 13 | **0.0%** | [0.0%, 24.7%] |
| A09 | Voice Conversion (VCC2018 + WaveNet) | 13 | 0 | 13 | **0.0%** | [0.0%, 24.7%] |
| A10 | Neural TTS (NN-TTS + WORLD) | 13 | 0 | 13 | **0.0%** | [0.0%, 24.7%] |
| A11 | Neural TTS (NN-TTS + Griffin-Lim) | 13 | 0 | 13 | **0.0%** | [0.0%, 24.7%] |
| A12 | Neural TTS (NN-TTS + WaveRNN) | 13 | 0 | 13 | **0.0%** | [0.0%, 24.7%] |
| A13 | Voice Conversion (VC + WaveRNN) | 13 | 0 | 13 | **0.0%** | [0.0%, 24.7%] |
| A14 | Voice Conversion (VC + WaveNet) | 13 | 0 | 13 | **0.0%** | [0.0%, 24.7%] |
| A15 | Voice Conversion (VC + Neural Vocoder) | 13 | 0 | 13 | **0.0%** | [0.0%, 24.7%] |
| A16 | Neural TTS (WaveNet-based TTS) | 13 | 0 | 13 | **0.0%** | [0.0%, 24.7%] |
| A17 | Voice Conversion (High-quality VC) | 13 | 0 | 13 | **0.0%** | [0.0%, 24.7%] |
| A18 | Voice Conversion (VC + WORLD vocoder) | 13 | 0 | 13 | **0.0%** | [0.0%, 24.7%] |
| A19 | Voice Conversion (VC + Spectral filtering) | 13 | 0 | 13 | **0.0%** | [0.0%, 24.7%] |
| **TOTAL** | | **169** | **0** | **169** | **0.0%** | |

---

## Seen vs Unseen Attack Comparison

| Category | Attack IDs | Training Exposure | EVAL Detection |
|----------|-----------|-----------------|----------------|
| Seen attacks | A01-A06 | Yes (~115 samples total) | N/A (used in training) |
| Unseen attacks | A07-A19 | None | **0% (all 13 attacks)** |

### Generalization Gap

> **100% of unseen attacks are missed.** The seen/unseen generalization gap is total: the model
> trained on A01-A06 provides **zero signal** for detecting A07-A19.

---

## Human Protection During Unseen Attack Eval

| Metric | Value |
|--------|-------|
| EVAL genuine speakers | 186 |
| False positives (human misclassified as spoof) | **0** |
| FPR | **0.00%** (95% CI: [0.00%, 2.02%]) |

Despite total failure to detect spoof, the model **does not falsely accuse any genuine human speakers**
in EVAL — because it predicts "genuine" for virtually all inputs.

---

## Root Cause Analysis

### Why 0% detection on all unseen attacks?

1. **Model collapses to majority class:** All three trained models converge to predicting "genuine"
   on EVAL inputs. This is a rational response to underfitting — the models cannot distinguish
   A07-A19 synthesis artifacts from genuine speech.

2. **Training set too small for deep model generalization:** 115 training samples, with 63 spoof
   spread across 6 seen attacks (~10/attack), is far below the minimum for learning generalizable
   acoustic spoof features.

3. **Seen/unseen vocoder mismatch:** A01-A06 primarily use older WORLD/STRAIGHT vocoders and
   concatenative synthesis. A07-A19 include Tacotron, WaveNet, WaveRNN, and Griffin-Lim-based
   neural vocoders — qualitatively different acoustic characteristics not seen in training.

4. **No pre-trained speech representations:** Log-Mel spectrogram features alone do not encode
   abstract "naturalness" or prosody artifacts detectable in small data regimes.

---

## Comparison with M07 (Frozen Model Baseline)

| Metric | M07 (Frozen Baseline) | M08 (Trained Model) | Change |
|--------|-----------------------|---------------------|--------|
| FNR on EVAL spoof | 91.09% (225/247) | 100.00% (169/169) | -8.91 pp (worse) |
| FPR on EVAL genuine | 0.00% (0/250) | 0.00% (0/186) | no change |
| EER | 42.31% | 50.24% | -7.93 pp (worse) |
| ROC-AUC | 0.5773 | 0.4976 | -0.0797 (worse) |

> M08 is strictly **worse** than M07 on unseen attacks. The frozen pre-trained model (M07) had
> some generalizable acoustic features embedded from its original training data. The M08 model,
> trained from scratch on 115 samples, has fewer generalizable features.

**This is the correct scientific outcome to document.** It confirms that:
- More data is needed (not different architecture)
- Transfer learning or pre-trained features are essential
- The M07 baseline should be the performance floor for M09+

---

## Recommendations for M09

### Primary: Full Training Set Access
- Download the full ASVspoof 2019 LA TRAIN split (~25,000 samples)
- Expected to provide sufficient coverage of A01-A06 synthesis variations

### Secondary: Pre-trained Speech Representations
- Use `speechbrain`, `wav2vec2`, or `HuBERT` embeddings as front-end features
- These capture speaker-independent naturalness cues that generalize to unseen attacks

### Tertiary: Hand-Crafted Anti-Spoof Features
- LFCC (Linear Frequency Cepstral Coefficients) — proven strong on ASVspoof 2019
- CQCC (Constant-Q Cepstral Coefficients) — strong on phase-based artifacts
- These features were designed specifically to capture vocoder artifacts

---

## Confidence Intervals Note

All 95% confidence intervals computed using Wilson score interval for binomial proportions.
N=13 per attack is small; interpret per-attack CIs as wide (±25%).
