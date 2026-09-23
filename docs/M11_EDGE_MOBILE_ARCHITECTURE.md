# M11 Edge / Mobile Architecture & Deployment Strategy

## 1. Executive Summary
Milestone 11 (M11) defines the edge/mobile deployment path for the SIH 2026 Voice Cloning Defense System. Given real-time audio latency requirements, browser compatibility across iOS and Android devices, and model inference requirements (AASIST 628K parameter neural network), the primary production path is established as **Mobile-Accessible Real-Time Streaming (PWA / Mobile Web)**.

```
+-----------------------------------+
|   MOBILE DEVICE / PHONE           |
|   (iOS Safari / Android Chrome)   |
|                                   |
|   Web Audio API Capture           |
|   (16kHz Int16 Mono PCM)          |
+-----------------------------------+
                  |
                  |  WebSocket Stream (WSS / WS)
                  v
+-----------------------------------+
|   BACKEND REAL-TIME PIPELINE      |
|   (FastAPI / Uvicorn Server)      |
|                                   |
|   StreamSession Rolling Buffer    |
|   (4.0s Window / 0.5s Hop)        |
|                                   |
|   Frozen M09 AASIST Model         |
|   (Platt Calibration T=0.30/0.70) |
|                                   |
|   StreamingRiskEngine             |
|   (Temporal Risk Smoothing N=5)   |
|                                   |
|   PreventionEngine                |
|   (Action Safety Policy)          |
+-----------------------------------+
                  |
                  |  Structured Evidence JSON
                  v
+-----------------------------------+
|   NEMOTRON ASSISTANT (Optional)   |
|   NVIDIA Nemotron 3.5 LLM         |
|   (Downstream Guidance Only)      |
+-----------------------------------+
```

---

## 2. Ingestion & Mobile Web Compatibility

### 2.1 Touch-Friendly Browser Interface
- **Responsive Layout**: Designed using CSS Flexbox/Grid with glassmorphic cards optimized for mobile screens (320px to 768px width) and desktop monitors.
- **Microphone Control**: Single tap `START LIVE STREAMING` / `STOP LISTENING` button with visual microphone activity meter.
- **Platform Support**: Works on iOS Safari, Android Chrome, Edge, and Desktop browsers with Web Audio API support.

### 2.2 Secure Audio Streaming
- **Transport**: Standard WebSocket (`ws://` or `wss://`).
- **Format**: 16-bit Int16 PCM audio samples (mono, 16,000 Hz).
- **Security Context**: Browsers mandate HTTPS / localhost for microphone access (`navigator.mediaDevices.getUserMedia`). Instructions for SSL proxying (e.g. ngrok or mkcert) are documented in `docs/M11_DEPLOYMENT_GUIDE.md`.

---

## 3. Edge Inference Export Feasibility Analysis

### 3.1 ONNX & TorchScript Assessment
- **Model**: Scaled AASIST (`m09_a_aasist_best.pt`, 628,082 parameters, 7.6 MB checkpoint size).
- **Operators**: AASIST utilizes RawNet graph convolutional sub-networks and complex spectrogram filterbanks.
- **Export Feasibility**: Backend-assisted inference achieves **8.97ms median latency** on CPU (RTF: 268.60x), making backend streaming highly optimized. Full ONNX runtime export for mobile WebAssembly / iOS CoreML remains optional for future on-device deployment.

---

## 4. Mobile System Boundaries & Platform Security Disclosures

> [!WARNING]
> **Mobile OS Permission Disclosures**:
> Modern mobile operating systems (iOS and Android) enforce strict sandboxing around system audio:
> 1. The prototype **CANNOT** intercept arbitrary incoming cellular calls, WhatsApp calls, or Telegram audio without explicit OS accessibility or system call framework integration.
> 2. Microphone access is granted **only** while the user actively opens the web application and taps `START LIVE STREAMING`.
> 3. Claims of universal background call interception across third-party apps are explicitly disclaimed.
