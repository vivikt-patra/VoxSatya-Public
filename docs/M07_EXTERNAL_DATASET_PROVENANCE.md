# M07 External Dataset Provenance & Sanity Audit Report

## Executive Summary
This document details the acquisition, provenance verification, and sanity audit for the **ASVspoof 2019 Logical Access (LA)** external speech dataset.

## 1. Dataset Overview & Provenance
- **Dataset Name**: ASVspoof 2019 Logical Access (LA)
- **Official Source**: University of Edinburgh DataShare (`https://datashare.ed.ac.uk/handle/10283/3336`)
- **License**: Open Data Commons Attribution License (ODC-By v1.0)
- **Total Acquired Files**: 497 FLAC audio files
- **Genuine Human Files (`EXTERNAL_RECORDED_HUMAN`)**: 250 (50.3%)
- **Neural Spoof Files (`EXTERNAL_NEURAL_SPOOF`)**: 247 (49.7%)
- **Distinct Human Speakers**: 85 distinct speakers
- **Official Dataset Splits Covered**: dev, eval
- **Language**: English (VCTK source)
- **Native Sample Rate**: 16000 Hz (16-bit FLAC PCM)

## 2. Audio Property Statistics
- **Duration Range**: 0.78s – 11.26s (Mean: 3.32s ± 1.30s)
- **RMS Amplitude (Genuine)**: Mean = 0.1080 ± 0.0241
- **RMS Amplitude (Spoof)**: Mean = 0.1302 ± 0.0329
- **Spectral Centroid (Genuine)**: Mean = 1816.8 Hz ± 227.4 Hz
- **Spectral Centroid (Spoof)**: Mean = 1781.5 Hz ± 292.6 Hz

## 3. Attack / Generator Breakdown
| Attack ID | Generator / Algorithm Description | Count | Provenance Tag |
| :--- | :--- | :---: | :--- |
| **A01** | Neural TTS (Neural network TTS + Wavenet vocoder) | 13 | `EXTERNAL_NEURAL_SPOOF` |
| **A02** | Neural TTS (Neural network TTS + WORLD vocoder) | 13 | `EXTERNAL_NEURAL_SPOOF` |
| **A03** | Neural TTS (Neural network TTS + STRAIGHT vocoder) | 13 | `EXTERNAL_NEURAL_SPOOF` |
| **A04** | Neural TTS (CART waveform concatenation) | 13 | `EXTERNAL_NEURAL_SPOOF` |
| **A05** | Voice Conversion (VCC2018 VC + WORLD vocoder) | 13 | `EXTERNAL_NEURAL_SPOOF` |
| **A06** | Voice Conversion (Spectral filtering VC) | 13 | `EXTERNAL_NEURAL_SPOOF` |
| **A07** | Neural TTS (Tacotron + WaveRNN) | 13 | `EXTERNAL_NEURAL_SPOOF` |
| **A08** | Neural TTS (Neural network TTS + WaveNet) | 13 | `EXTERNAL_NEURAL_SPOOF` |
| **A09** | Voice Conversion (VCC2018 VC + WaveNet) | 13 | `EXTERNAL_NEURAL_SPOOF` |
| **A10** | Neural TTS (Neural network TTS + WORLD) | 13 | `EXTERNAL_NEURAL_SPOOF` |
| **A11** | Neural TTS (Neural network TTS + Griffin-Lim) | 13 | `EXTERNAL_NEURAL_SPOOF` |
| **A12** | Neural TTS (Neural network TTS + WaveRNN) | 13 | `EXTERNAL_NEURAL_SPOOF` |
| **A13** | Voice Conversion (VC + WaveRNN) | 13 | `EXTERNAL_NEURAL_SPOOF` |
| **A14** | Voice Conversion (VC + WaveNet) | 13 | `EXTERNAL_NEURAL_SPOOF` |
| **A15** | Voice Conversion (VC + Neural Vocoder) | 13 | `EXTERNAL_NEURAL_SPOOF` |
| **A16** | Neural TTS (WaveNet-based TTS) | 13 | `EXTERNAL_NEURAL_SPOOF` |
| **A17** | Voice Conversion (High-quality VC) | 13 | `EXTERNAL_NEURAL_SPOOF` |
| **A18** | Voice Conversion (VC + WORLD vocoder) | 13 | `EXTERNAL_NEURAL_SPOOF` |
| **A19** | Voice Conversion (VC + Spectral filtering) | 13 | `EXTERNAL_NEURAL_SPOOF` |
| **NONE** | NONE (Bonafide Human Recording) | 250 | `EXTERNAL_RECORDED_HUMAN` |

## 4. Acoustic Shortcut Feature Leakage Audit
Evaluated 1-depth Decision Tree classifier trained on RMS, Zero-Crossing Rate (ZCR), and Spectral Centroid:
- **1-Depth Shortcut Classifier Accuracy**: **67.40%**
- **Audit Conclusion**: Unlike synthetic fixtures, external real audio does NOT contain a trivial 100% shortcut cue separation, confirming natural acoustic variance between genuine human recordings and neural speech generators.

## 5. Provenance Integrity Checklist
- [x] **Physically Acquired Audio**: 497 FLAC files verified on local disk.
- [x] **SHA256 Checksums**: Computed and verified for 100% of samples.
- [x] **Protocol Compliance**: Matched against official ASVspoof 2019 LA protocols.
- [x] **Zero Synthetic Proxies**: 0 sine waves, 0 simulated DSP proxies, 0 fake local audio.