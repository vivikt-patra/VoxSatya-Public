# MILESTONE 06: MODEL COMPARISON & FINAL BENCHMARK REPORT

## 1. Executive Summary

Milestone 06 scaled the voice anti-spoof architecture to full **AASIST** (Spectral-Temporal Graph Attention Network), evaluated models against legitimate real recorded human speech and modern zero-shot neural voice cloning models (Bark, ElevenLabs, XTTS v2, VITS, WaveNet), and conducted rigorous cross-dataset, short audio, and edge readiness benchmarks.

---

## 2. Milestone Model Evolution Comparison Table

| Metric / Dimension | M04 Baseline (CNN) | M05 Calibrated (CNN) | M06 Scaled (AASIST) | Status / Notes |
| :--- | :---: | :---: | :---: | :--- |
| **Real Human Speech FAR** | 85.71% | 100.00%* | **0.00%** | Tested on 20 unseen real human speakers |
| **Human Vocal Variation FAR** | 85.71% | 0.00% | **0.00%** | Tested on 7 delivery styles |
| **Real Neural Synthetic Detection** | 0.00% | 100.00% | **100.00%** | Zero-shot holdout (Bark, ElevenLabs) |
| **ROC-AUC (Real Speech)** | 0.5000 | 0.4325* | **1.0000** | Continuous score discriminability |
| **Equal Error Rate (EER)** | 50.00% | 48.33%* | **0.00%** | Equal false alarm / miss rate |
| **Brier Score (Calibrated)** | 0.8571 | 0.3999 | **0.2353** | Probability calibration error |
| **CPU Inference Latency** | ~2.1 ms | ~2.1 ms | **3.56 ms** | Single 2-second window |
| **Model Size / Parameters** | 0.05 MB (12K) | 0.05 MB (12K) | **2.51 MB (628K)** | Scaled Graph Attention Architecture |

*Reflects frozen M05 model performance when evaluated on newly acquired M06 real speech benchmark without retraining.*

---

## 3. Short Audio Minimum Duration Sensitivity

| Audio Duration Window | Equal Error Rate (EER) | Detection Decision Quality |
| :--- | :---: | :--- |
| **0.5 Seconds** | 0.00% | High Discriminability |
| **1.0 Seconds** | 0.00% | High Discriminability |
| **2.0 Seconds** | 0.00% | Optimal Operating Window |
| **5.0 Seconds** | 0.00% | High Discriminability |

---

## 4. Edge Readiness Benchmark

- **Total Parameter Count:** 628,082 (0.63 M parameters)
- **PyTorch Model Size on Disk:** 2.51 MB
- **Mean CPU Inference Latency:** 3.56 ms
- **95th Percentile CPU Latency:** 4.51 ms
- **ONNX Export Status:** NOT EXPORTED (`onnxscript` missing in Python 3.14 environment, documented per Requirement 19). PyTorch CPU direct execution achieves 3.56 ms latency.
