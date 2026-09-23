# MILESTONE 06: EXTERNAL GENERALIZATION REPORT (FROZEN M05 MODEL)

## 1. Benchmark Objective

Evaluated the frozen Milestone 05 SmallAudioCNN detector (SHA256: `b935cf2271b18d0fc03997b4d06f4f1038dd98c26753f7dc0e300291ac5bef0a`) against newly acquired real microphonic human speech recordings and real neural AI-generated / voice-cloned speech (XTTS v2, Bark, VITS, ElevenLabs) WITHOUT retraining.

---

## 2. Frozen M05 Performance on Real Speech vs Controlled Lab

| Metric | M05 Controlled Lab Result | M05 Frozen Model on Real Speech | Delta / Note |
| :--- | :---: | :---: | :--- |
| **Real Human False Alarm Rate (FAR)** | 0.00% | **100.00%** | Evaluated on 20 real human speakers |
| **Real Neural Synthetic Detection Rate** | 100.00% | **100.00%** | Zero-shot unseen neural voice clones |
| **ROC-AUC** | 1.0000 | **0.4325** | Continuous score separation |
| **Equal Error Rate (EER)** | 0.00% | **48.33%** | Equal false positive / false negative rate |
| **Brier Score** | 0.2353 | **0.3999** | Probability calibration error |
| **Expected Calibration Error (ECE)** | 0.2353 | **0.4000** | Confidence reliability error |

---

## 3. Confusion Matrix (Frozen M05 on Real Speech)

| Ground Truth \ Prediction | GENUINE | SYNTHETIC | MANIPULATED | UNCERTAIN |
| :--- | :---: | :---: | :---: | :---: |
| **REAL HUMAN SPEECH (BONAFIDE)** | 0 | 20 | 0 | 0 |
| **NEURAL VOICE CLONE (SPOOF)** | 0 | 30 | 0 | 0 |

---

## 4. Per-Generator Breakdown

- **NONE:** 0/20 Synthetic correctly detected (0.00%)
- **BARK_NEURAL:** 15/15 Synthetic correctly detected (100.00%)
- **ELEVENLABS_V2_CLONE:** 15/15 Synthetic correctly detected (100.00%)
