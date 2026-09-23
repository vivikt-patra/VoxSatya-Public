# Milestone 05 — Pipeline Discrepancy Audit & Initial State Record

## 1. Initial State & Model Configuration Record

- **Git Branch:** `experiment/M05-calibration-robust-training`
- **Starting Checkpoint Tag:** `checkpoint/M04`
- **Baseline CNN Checkpoint:** `models/baseline/cnn/best_model.pt`
- **CNN Checkpoint SHA256:** `0e8305ad74b06440ab84663af30914dcb0c4ac91ce235fafe681889f9d71d52e`
- **Baseline Model Parameters:**
  - Sample Rate: 16,000 Hz
  - Audio Duration Window: 3.0 seconds (48,000 samples)
  - STFT Window Size ($N_{fft}$): 512
  - Hop Length: 160 samples (10 ms)
  - Mel Filter Banks ($N_{mels}$): 64
- **Decision Engine Thresholds (M04):**
  - `synthetic_threshold`: 0.60
  - `manipulation_threshold`: 0.50
  - `uncertainty_threshold`: 0.20
  - `snr_min_threshold_db`: 3.0 dB
  - `max_clipping_ratio`: 0.08 (8.0%)

---

## 2. M02 → M04 Pipeline Discrepancy Investigation

### The Question:
Why did M02 report 20/20 detected synthetic samples, while M04 reported 0/20 SYNTHETIC and 20/20 UNCERTAIN, along with an 85.71% false alarm rate on genuine human vocal variations?

### Comprehensive Root-Cause Analysis:

#### 1. Synthetic Signal Definition Discrepancy
- **M02 Training Signal Generation (`acquire_baseline_dataset.py`):**
  - **Bonafide Audio:** Generated as a pure 3-tone harmonic sine wave (`0.4*sin(f0) + 0.2*sin(2*f0) + 0.1*sin(3*f0)`).
  - **Spoof Audio:** Generated with phase modulation (Tacotron 2 simulation), pitch jitter, or high-frequency carrier perturbations.
- **M04 Synthetic Audio Generation (`benchmark_m04_manipulation.py`):**
  - Synthetic samples were created using a simple 2-tone pure harmonic sine wave (`0.4*sin(440) + 0.3*sin(880)`).
  - Because `syn_signal` in M04 was structured as a pure harmonic tone, the M02-trained CNN recognized it as **Bonafide speech** (predicting `spoof_probability ≈ 0.51`).
  - Because `abs(0.51 - 0.50) = 0.01 < 0.20` (`uncertainty_threshold`), the MultiClassDecisionEngine mapped all 20 synthetic samples to `UNCERTAIN`.

#### 2. Human Variation False-Alarm Root Cause
- **M04 Human Vocal Variation Generation (`human_variation_dataset.py`):**
  - Human vocal variations (pitch shifts, fast/slow pacing, formant changes) introduced non-standard spectral features relative to M02's static sine wave training signals.
  - The uncalibrated CNN interpreted pitch shifts and formant modulations as synthetic generator artifacts, outputting `spoof_probability ≈ 0.85 – 0.95`.
  - The Decision Engine consequently classified 30/35 genuine human variation samples as `SYNTHETIC` (85.71% False Alarm Rate).

---

## 3. Single Audio Sample Execution Trace

### Trace of Synthetic Sample `syn_sample_00.wav` in M04 Pipeline:

1. **Raw Audio WAV:** 3.0s, 16,000 Hz float32 waveform (`0.4*sin(440) + 0.3*sin(880)`).
2. **Preprocessing (`load_and_preprocess_audio`):** Resampled to 16,000 Hz, peak normalized to 0.95, shape `(48000,)`.
3. **Feature Extractor (`extract_log_mel`):** Log-Mel Spectrogram shape `(1, 64, 301)`.
4. **CNN Forward Pass (`SmallAudioCNN`):** Output raw logit `0.04`, Sigmoid output `0.5100`.
5. **Raw Spoof Probability:** `spoof_probability = 0.5100`.
6. **Signal Quality Assessment (`compute_signal_quality`):** `estimated_snr_db = 32.4 dB`, `clipping_ratio = 0.0`.
7. **Voice Manipulation Detector (`VoiceManipulationDetector`):** `phase_discontinuity = 0.12 rad`, `manipulation_score = 0.00`.
8. **Decision Engine (`MultiClassDecisionEngine`):**
   - `ambiguity = abs(0.5100 - 0.50) = 0.01`
   - `ambiguity (0.01) < uncertainty_threshold (0.20)` and `detector_prob (0.51) < synthetic_threshold (0.60)`
   - Result: `UNCERTAIN`
9. **Final Output Classification:** `UNCERTAIN`.

---

## 4. Remediation Plan in Milestone 05

1. **Canonical Preprocessing Engine (`canonical_pipeline.py`):** Enforce 100% parity across training, testing, and API inference.
2. **Class Semantics Verification:** Lock class index `0 = BONAFIDE`, `1 = SPOOF`, `spoof_probability = P(SPOOF)`.
3. **Realistic Audio & Vocal Generator Expansion:** Replace toy sine wave signals with speech synthesis modeling (glottal pulses, formant shifts, acoustic resonances).
4. **Probability Calibration:** Apply Temperature Scaling on validation logits.
5. **Robust Retraining:** Retrain models with channel augmentation (noise, RIR, codec) and pitch/pacing variations to teach the model true bonafide vs spoof invariance.
