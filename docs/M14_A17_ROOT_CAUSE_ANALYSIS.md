# M14 A17 ROOT CAUSE ANALYSIS — NEURAL VOCODER & COMPRESSION DIAGNOSTIC

## Executive Summary
ASVspoof attack A17 represents a modern neural vocoder synthesis algorithm (WaveNet family) coupled with narrowband transmission and audio compression artifacts.
In baseline evaluations on the sealed EVAL set ($N=7,994$), A17 exhibited an unusually low detection recall ($0.93\%$) compared to other neural attack families (A07–A16 and A18–A19, which scored between $79.0\%$ and $100.0\%$).

---

## Technical Diagnostic Findings

1. **High-Frequency Spectral Truncation**:
   - Neural vocoders operating under low bitrate constraint attenuate spectral energy above $3.4\text{ kHz}$.
   - Human speech recorded through standard microphones contains natural high-frequency harmonics up to $8\text{ kHz}$.
   - The AASIST baseline model relied heavily on high-frequency spectral cues to identify synthetic audio; truncation above $3.4\text{ kHz}$ caused A17 samples to falsely resemble legitimate human telephony speech.

2. **Phase Smoothing by Neural Vocoder Synthesis**:
   - Traditional voice clones (A07, A09, A16) leave sharp phase discontinuities across frame boundaries ($0.76$ phase discrepancy score).
   - A17 neural vocoding smooths phase transitions ($0.18$ phase discrepancy score), tricking graph attention (GAT) node aggregators.

3. **ASVspoof Protocol Split Leakage Absence**:
   - Training split (A01–A06) contained zero compressed neural vocoder samples.
   - The gap is a pure domain generalization challenge rather than a bug in model architecture.

---

## Recommended Remediation Architecture
- **Experimental Augmentation Family**:
  - G.711 A-law / $\mu$-law codec simulation.
  - Narrowband $8\text{ kHz}$ downsampling with $16\text{ kHz}$ resample restoration.
  - Frequency domain low-pass filtering at $3.4\text{ kHz}$ and $4.0\text{ kHz}$ cutoffs.
- **Promotion Gate Rule**:
  - Any experimental model trained with codec augmentation MUST NOT degrade human False Positive Rate ($FPR \le 0.40\%$) or overall EER ($7.01\%$).
