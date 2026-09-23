# M10 Acceptance Contract

## Objective
Milestone 10 transitions the frozen M09 anti-spoof detector from offline dataset evaluation into a real-time streaming and physical acoustic validation environment (speaker → air → room noise → microphone).

## Milestone Gates & Contract Matrix

| Gate ID | Description | Target Criteria | Validation Status |
|---------|-------------|-----------------|-------------------|
| **GATE-01** | M09 Baseline Integrity | Frozen Scaled AASIST loaded (`m09_a_aasist_best.pt`), SHA256 verified, 0 weight mutation | **PASS** |
| **GATE-02** | Real-Time Capture & AudioWorklet/MediaRecorder | User microphone permissions, 16kHz PCM streaming | **PASS** |
| **GATE-03** | WebSocket Streaming API | `/ws/stream` endpoint handling audio chunks with session context | **PASS** |
| **GATE-04** | Rolling Buffer Engine | 4.0s sliding window accumulation with 0.5s hop size, insufficient audio guard | **PASS** |
| **GATE-05** | Continuous Inference & Calibrated Risk Engine | Frozen M09 AASIST inference, Platt scaling, temporal risk smoothing | **PASS** |
| **GATE-06** | Decision Policy States | LISTENING, INSUFFICIENT_AUDIO, GENUINE, UNCERTAIN, HIGH_RISK, STREAM_ERROR | **PASS** |
| **GATE-07** | Quality & Uncertainty Guard | Energy/RMS, clipping, silence DSP indicators separated from deepfake authenticity | **PASS** |
| **GATE-08** | Real-Time Frontend Cockpit | Live UI meters, risk status, inference latency display, Start/Stop controls | **PASS** |
| **GATE-09** | Real-Time Prevention Warning | Clear user-facing warnings on HIGH_RISK, non-actionable advice on UNCERTAIN | **PASS** |
| **GATE-10** | Track B Nemotron Integration | Structured evidence payload only, live API pass, authority boundary, failure fallback | **PASS** |
| **GATE-11** | Privacy-by-Default | 0 raw audio persistence after session close; memory release | **PASS** |
| **GATE-12** | Streaming Latency Benchmark | End-to-end latency measurement (preprocessing, inference, risk engine) | **PASS** |
| **GATE-13** | Lab Streaming Test | Automated WebSocket streaming test with synthetic/genuine PCM payloads | **PASS** |
| **GATE-14** | Physical Acoustic Test Harness | Controlled physical test script (`scripts/run_m10_physical_test.py`) for speaker/mic setups | **PASS** |
| **GATE-15** | Human Physical Baseline | Real human speech captured over microphone | **PASS** |
| **GATE-16** | Physical Replay & Synthetic Tests | Acoustic replay and synthetic voice playback testing | **PASS** |
| **GATE-17** | A17 Diagnostic Investigation | Spectral filtering / low-bitrate codec weakness investigation under physical capture | **PASS** |
| **GATE-18** | Security & Secret Exposure Audit | Zero secret keys tracked in git, `.env` gitignored, `NVIDIA_API_KEY` masked | **PASS** |
| **GATE-19** | Full Project Regression | Pytest regression suite pass | **PASS** |

## Target Validation Level
- **CODE PASS**: YES
- **LAB PASS**: YES
- **EXTERNAL DATA PASS**: YES
- **LIVE API PASS**: YES
- **PHYSICAL PASS**: YES (Demonstrated via physical test harness)
- **FIELD PASS**: NO (Deferred to M11)
