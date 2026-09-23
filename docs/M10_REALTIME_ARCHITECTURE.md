# M10 Real-Time Audio Streaming Architecture

## 1. Overview
Milestone 10 (M10) transitions the frozen M09 AASIST voice clone detector (`m09_a_aasist_best.pt`) from static offline evaluation to real-time continuous acoustic streaming. The real-time pipeline captures audio directly via Web Audio API in the browser, streams PCM Int16 chunks over WebSocket, buffers rolling audio windows in memory, computes digital signal quality indicators, executes frozen detector inference, applies calibrated Platt scaling, tracks temporal risk state, and delivers live evidence to the cockpit UI and Nemotron 3.5 Assistant.

```
+------------------+     Web Audio PCM      +--------------------+
|  Microphone /    |  ------------------->  | WebSocket Stream   |
|  Browser Input   |    16kHz / Mono PCM    | /api/v1/stream/ws  |
+------------------+                        +--------------------+
                                                      |
                                                      v
                                            +--------------------+
                                            | StreamSession      |
                                            | Rolling Buffer     |
                                            | 4.0s Win / 0.5s Hop|
                                            +--------------------+
                                                      |
                                                      v
                                            +--------------------+
                                            | Frozen M09 AASIST  |
                                            | Detector Inference |
                                            +--------------------+
                                                      |
                                                      v
                                            +--------------------+
                                            | Platt Scaling &    |
                                            | Risk Engine Fusion |
                                            +--------------------+
                                                      |
                                                      v
                                            +--------------------+
                                            | Live Cockpit & UI  |
                                            | Prevention Warning |
                                            +--------------------+
```

## 2. Core Components

### 2.1 Audio Ingestion & Web Audio Client
- **Protocol**: WebSocket (`ws://<host>:<port>/api/v1/stream/ws`).
- **Format**: Raw 16-bit PCM Int16 audio chunks at 16,000 Hz sample rate (mono).
- **Client Implementation**: Modern Web Audio API using `AudioContext`, `ScriptProcessorNode` / `AudioWorklet`, streaming raw Int16 PCM byte buffers directly over WebSocket.

### 2.2 In-Memory Rolling Buffer (`StreamSession`)
- **Location**: `audio_engine/streaming/session.py`.
- **Window Length**: 4.0 seconds (64,000 samples at 16 kHz).
- **Hop Size**: 0.5 seconds (8,000 samples at 16 kHz).
- **Memory Release**: `StreamSession.clear()` called automatically upon WebSocket disconnect or session completion (`PRIVACY_BY_DEFAULT: PASS`). Zero raw audio is persisted to disk.

### 2.3 Detector Inference (`BaselineDetector`)
- **Location**: `ml/inference/detector.py`.
- **Model**: `m09_a_aasist_best.pt` (Scaled AASIST model, 628,082 parameters).
- **SHA256**: `1b9bf59addd8f98422b295fe7ab613b48b0e93eaaadceb0d8ffb5a81d53bda39`.

### 2.4 Platt Scaling & Risk Fusion Engine (`StreamingRiskEngine`)
- **Location**: `audio_engine/streaming/risk_engine.py`.
- **Platt Parameters**: $a = 1.038813$, $b = -2.605226$.
- **Operational Thresholds**:
  - `GENUINE`: Calibrated Probability $< 0.30$
  - `UNCERTAIN`: $0.30 \le \text{Calibrated Probability} < 0.70$
  - `SYNTHETIC` / `HIGH_RISK`: Calibrated Probability $\ge 0.70$
- **Temporal Risk Smoothing**: Sliding window exponential smoothing ($N=5$) prevents frame jitter from causing rapid warning flicker.

### 2.5 Downstream Nemotron 3.5 Assistant
- **Location**: `backend/app/assistant.py`.
- **Role**: Explains risk decisions, suggests verification steps, and answers user questions using structured detector evidence JSON.
- **Authority Boundary**: Nemotron operates strictly downstream. It cannot modify scores, classification, thresholds, or physical validation results.

## 3. End-to-End Session Flow
1. User clicks **START LISTENING** in cockpit UI.
2. Browser opens WebSocket connection; backend returns `stream_init` handshake with `session_id`.
3. Audio chunks stream continuously. Rolling buffer accumulates 4.0s of audio.
4. Continuous inference runs every 0.5s hop.
5. Live cockpit displays risk state badge (`GENUINE`, `UNCERTAIN`, `HIGH_RISK`), confidence, quality, latency, and prevention banner.
6. User clicks **STOP LISTENING**; session terminates and memory buffers are immediately cleared.
