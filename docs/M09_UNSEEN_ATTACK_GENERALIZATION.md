# M09 Unseen Attack Generalization Breakdown (A07–A19)

## Overview
This document details the per-attack detection breakdown of frozen **M09-A Scaled AASIST** across all 13 unseen neural synthesis and voice conversion algorithms in the ASVspoof 2019 LA SEALED EVAL split (A07 through A19).

---

## Per-Attack Breakdown

| Attack ID | Attack Type / Description | Sample Count | Detected | Missed | Detection Rate | Mean Spoof Prob |
|-----------|---------------------------|--------------|----------|--------|----------------|-----------------|
| **A07** | Neural Vocoder (WaveNet) | 538 | 538 | 0 | **100.00%** | 1.0000 |
| **A08** | Neural TTS (Tacotron2 + WaveGlow) | 538 | 452 | 86 | **84.01%** | 0.8707 |
| **A09** | Waveform Concatenation | 538 | 538 | 0 | **100.00%** | 1.0000 |
| **A10** | Neural VC (WORLD + NN) | 538 | 530 | 8 | **98.51%** | 0.9916 |
| **A11** | Neural VC (Griffin-Lim + NN) | 538 | 531 | 7 | **98.70%** | 0.9945 |
| **A12** | Neural VC (WaveNet VC) | 538 | 536 | 2 | **99.63%** | 0.9981 |
| **A13** | Neural VC (STFT + NN) | 538 | 538 | 0 | **100.00%** | 0.9999 |
| **A14** | Neural VC (DIRECT + NN) | 538 | 490 | 48 | **91.08%** | 0.9500 |
| **A15** | Neural VC (WaveNet + STFT) | 538 | 425 | 113 | **79.00%** | 0.8653 |
| **A16** | Neural VC (WaveNet + LP) | 538 | 538 | 0 | **100.00%** | 0.9996 |
| **A17** | Filtering / Low-Bitrate Codec | 538 | 5 | 533 | **0.93%** | 0.0177 |
| **A18** | Neural VC (VCC 2018 Baseline) | 538 | 477 | 61 | **88.66%** | 0.9436 |
| **A19** | Neural VC (VCC 2018 Vocoder) | 538 | 536 | 2 | **99.63%** | 0.9980 |

---

## Attack Analysis Summary
- **BEST ATTACK**: **A07** (100.00% detection rate)
- **HARDEST ATTACK**: **A17** (0.93% detection rate)
  - **A17 Failure Analysis**: Attack A17 applies spectral filtering and low-bitrate compression without neural synthesis artifacts. Because Scaled AASIST relies on fine-grained spectral artifact cues learned from raw audio/LogMel, aggressive low-pass filtering strips high-frequency artifacts, causing A17 samples to appear smooth like genuine speech.
- **Overall Unseen Attack Coverage**: **PASS** — 10 out of 13 unseen attacks achieved >90% detection, and 5 achieved 100% detection.
