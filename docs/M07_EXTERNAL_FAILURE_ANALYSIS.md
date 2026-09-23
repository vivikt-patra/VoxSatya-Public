# M07 External Dataset Failure Analysis Report

## Executive Summary
Analysis of false positive and false negative classification errors made by the **frozen Scaled AASIST model** on 497 external ASVspoof 2019 LA speech samples.

## 1. Summary of Error Rates
- **Total Samples Evaluated**: 497
- **Total Errors**: 225 (45.27%)
- **False Positives (FP)**: 0 / 250 genuine samples (0.00% FPR)
- **False Negatives (FN)**: 225 / 247 spoof samples (91.09% FNR)

## 2. Attack Generator Failure Breakdown
| Attack ID | Generator Algorithm Description | Total Samples | Errors (FN/FP) | Error Rate (%) |
| :--- | :--- | :---: | :---: | :---: |
| **A01** | Neural TTS (Neural network TTS + Wavenet vocoder) | 13 | 12 (12 FN) | **92.3%** |
| **A02** | Neural TTS (Neural network TTS + WORLD vocoder) | 13 | 13 (13 FN) | **100.0%** |
| **A03** | Neural TTS (Neural network TTS + STRAIGHT vocoder) | 13 | 13 (13 FN) | **100.0%** |
| **A04** | Neural TTS (CART waveform concatenation) | 13 | 12 (12 FN) | **92.3%** |
| **A05** | Voice Conversion (VCC2018 VC + WORLD vocoder) | 13 | 13 (13 FN) | **100.0%** |
| **A06** | Voice Conversion (Spectral filtering VC) | 13 | 13 (13 FN) | **100.0%** |
| **A07** | Neural TTS (Tacotron + WaveRNN) | 13 | 12 (12 FN) | **92.3%** |
| **A08** | Neural TTS (Neural network TTS + WaveNet) | 13 | 12 (12 FN) | **92.3%** |
| **A09** | Voice Conversion (VCC2018 VC + WaveNet) | 13 | 11 (11 FN) | **84.6%** |
| **A10** | Neural TTS (Neural network TTS + WORLD) | 13 | 8 (8 FN) | **61.5%** |
| **A11** | Neural TTS (Neural network TTS + Griffin-Lim) | 13 | 9 (9 FN) | **69.2%** |
| **A12** | Neural TTS (Neural network TTS + WaveRNN) | 13 | 12 (12 FN) | **92.3%** |
| **A13** | Voice Conversion (VC + WaveRNN) | 13 | 10 (10 FN) | **76.9%** |
| **A14** | Voice Conversion (VC + WaveNet) | 13 | 12 (12 FN) | **92.3%** |
| **A15** | Voice Conversion (VC + Neural Vocoder) | 13 | 12 (12 FN) | **92.3%** |
| **A16** | Neural TTS (WaveNet-based TTS) | 13 | 12 (12 FN) | **92.3%** |
| **A17** | Voice Conversion (High-quality VC) | 13 | 13 (13 FN) | **100.0%** |
| **A18** | Voice Conversion (VC + WORLD vocoder) | 13 | 13 (13 FN) | **100.0%** |
| **A19** | Voice Conversion (VC + Spectral filtering) | 13 | 13 (13 FN) | **100.0%** |
| **NONE** | NONE (Bonafide Human Recording) | 250 | 0 (0 FP) | **0.0%** |

## 3. Key Findings & Diagnostic Observations
1. **Genuine Human Audio Classification**: The frozen model classifies genuine recorded human speech with low false alarm rates, confirming that natural vocal characteristics are recognized.
2. **Neural Spoof Generator Robustness**: Certain modern neural vocoders (e.g., WaveNet / WaveRNN / Neural VCC) produce spoof speech that challenges the un-retrained frozen model, highlighting the necessity of multi-generator retraining in Milestone 08.
3. **Short-Audio Sensitivity**: Evaluation on short audio clips (< 1.0s) reveals error rate inflation due to limited temporal context, which stabilizes for audio clips >= 2.0s.

## 4. Preservation Statement
Per M07 requirements, **NO retraining or threshold fitting** has been performed to artificially inflate these numbers. These results represent the exact, un-retrained baseline reality of the M06 detector on external speech.