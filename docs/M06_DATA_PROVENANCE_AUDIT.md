# MILESTONE 06: DATA PROVENANCE AUDIT

## 1. M05 Dataset Provenance Classification

Prior to acquiring external real-speech benchmarks for Milestone 06, every dataset item from Milestone 05 was audited and classified into four provenance categories:
1. `REAL_RECORDED_HUMAN`: Microphonic human vocal recordings from real physical speakers.
2. `REAL_TTS_OR_VOICE_CLONE`: Real-world neural text-to-speech or zero-shot voice cloning model outputs.
3. `SIMULATED_AUDIO`: Controlled algorithmic/mathematical audio synthesis models (e.g., formant vocal synthesis, DSP transformations, harmonic vocoder synthesis).
4. `TEST_FIXTURE`: Unit test smoke fixtures (e.g. 1-second sine waves, 0-byte silent buffers).

---

## 2. Milestone 05 Provenance Audit Ledger

| Dataset Split | Total Items | REAL_RECORDED_HUMAN | REAL_TTS_OR_VOICE_CLONE | SIMULATED_AUDIO | TEST_FIXTURE | Primary Data Origin / Generator |
| :--- | :---: | :---: | :---: | :---: | :---: | :--- |
| **M05 Training Split** | 140 | 0 | 0 | 140 | 0 | Formant Vocal Model (70) + Vocoder `GEN_01`..`03` (40) + DSP Transforms (30) |
| **M05 Validation Split** | 70 | 0 | 0 | 70 | 0 | Formant Vocal Model (35) + Vocoder `GEN_01`..`03` (20) + DSP Transforms (15) |
| **M05 Test Split (Held-out)** | 70 | 0 | 0 | 70 | 0 | Formant Vocal Model (35) + Unseen Vocoder `GEN_04`..`05` (20) + DSP Transforms (15) |
| **TOTAL M05 DATASET** | **280** | **0 (0.0%)** | **0 (0.0%)** | **280 (100.0%)** | **0 (0.0%)** | **100% Controlled Synthetic/Simulated Audio** |

---

## 3. Findings & Imperatives for Milestone 06

- **Finding:** The Milestone 05 dataset consisted entirely of controlled simulated audio signals ($0.0\%$ real human recordings, $0.0\%$ real neural deepfake recordings). While essential for verifying pipeline numerical stability and fixing logit explosion flaws, M05 results reflect controlled-lab validity only.
- **Imperative:** Milestone 06 must evaluate models against **legitimate real recorded human speech** and **real neural AI-generated / voice-cloned speech** (ASVspoof protocols, XTTS v2, Bark, VITS, ElevenLabs, WaveNet).
- **Rule:** No simulated audio signal or unit test fixture may be presented as real-world deepfake evidence in Milestone 06.
