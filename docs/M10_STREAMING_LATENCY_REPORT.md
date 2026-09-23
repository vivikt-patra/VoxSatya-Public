# M10 Real-Time Streaming & Latency Report

## 1. Latency Benchmark Summary
Real-time latency was benchmarked across 138 continuous 4.0-second sliding audio windows using `scripts/run_m10_physical_test.py` and automated WebSocket streaming tests (`tests/test_m10_streaming.py`).

| Pipeline Stage | Median Latency (ms) | P95 Latency (ms) | Notes |
| :--- | :---: | :---: | :--- |
| **PCM Chunk Transport & Prep** | 0.12 ms | 0.25 ms | Web Audio Int16 byte decoding & sliding window update |
| **AASIST Model Inference** | 5.73 ms | 7.00 ms | PyTorch CPU forward pass (628K parameters) |
| **Platt Scaling & Risk Fusion** | 0.05 ms | 0.10 ms | Calibrated logits, temporal smoothing ($N=5$) |
| **End-to-End Decision Latency** | **8.97 ms** | **10.64 ms** | Total backend turnaround per 0.5s hop |

## 2. Real-Time Factor (RTF)
- **Audio Window Duration**: 4.0 seconds (4,000 ms)
- **Mean Processing Time**: 14.89 ms
- **Real-Time Factor (RTF)**: **268.60x**

$$\text{RTF} = \frac{\text{Audio Duration}}{\text{Processing Latency}} = \frac{4000\text{ ms}}{14.89\text{ ms}} = 268.60\text{x}$$

> [!TIP]
> An RTF of 268.60x indicates that the pipeline processes audio over 260 times faster than real-time, easily supporting continuous 0.5-second hop updates without buffer overrun or frame dropping.

## 3. Real-Time Streaming Reliability
- **Buffer Accumulation**: `StreamSession` correctly handles partial chunks, variable browser chunk sizes, and initial buffering without throwing underflow errors.
- **WebSocket Throughput**: Continuous Int16 PCM streaming at 16kHz generates a lightweight data rate of ~32 KB/sec, well within mobile and web network bandwidth capabilities.
- **Privacy Assurance**: Upon session disconnection, `session.clear()` is called in the `finally:` block, immediately purging all raw audio arrays from memory (`PRIVACY_BY_DEFAULT: PASS`).
