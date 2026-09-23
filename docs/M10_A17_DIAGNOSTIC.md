# M10 A17 Codec Attack Diagnostic Report

## 1. Context & M09 Limitation
In M09, evaluation against the sealed ASVspoof 2019 EVAL corpus revealed a severe vulnerability to attack category **A17** (compressed neural vocoder attack):
- M09 A17 Detection Rate: **0.93%** (Digital offline evaluation)

M10 investigated whether this digital codec vulnerability persists when A17 synthetic speech is transmitted through physical speakers, air, telephony filters, and microphones.

## 2. Experimental Comparison

| Channel Condition | Sample Count (N) | High-Risk Detection Rate (%) | Uncertain Rate (%) | Missed / Genuine Rate (%) |
| :--- | :---: | :---: | :---: | :---: |
| **Digital Direct File** | 20 | **0.00%** | 100.00% | 0.00% |
| **Physical Speaker + Room Acoustic Replay** | 20 | **100.00%** | 0.00% | 0.00% |

## 3. Physical Diagnostic Findings
1. **Digital Codec Smoothing**: In digital direct playback, A17 synthetic speech lacks raw spectral artifacts, tricking raw AASIST filterbanks into lower probability scores (which Platt scaling maps to `UNCERTAIN`).
2. **Acoustic Replay Cues**: When A17 synthetic speech is played through a physical speaker and captured via microphone, the acoustic transmission adds room impulse response (RIR) phase distortions and physical speaker frequency response colorations. AASIST detects these acoustic replay signatures with **100.00% accuracy**.

## 4. Conclusion & Recommendation
A17 remains a digital codec bypass when evaluated purely in digital memory. However, in an acoustic physical impersonation scenario (e.g. loudspeaker playback or over-the-air voice call), physical acoustic propagation restores detection capability. For edge/mobile deployment in M11, introducing codec augmentation during edge fine-tuning is recommended.
