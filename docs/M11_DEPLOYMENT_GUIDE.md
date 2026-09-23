# M11 System Deployment & Local Setup Guide

## 1. Environment & Prerequisites
- **Python**: Version 3.11, 3.12, 3.13, or 3.14.
- **Dependencies**: Install via standard pip:
  ```bash
  pip install torch torchaudio numpy scipy soundfile fastapi uvicorn pydantic python-multipart python-dotenv requests pytest
  ```
- **Model Checkpoint**: Ensure `m09_a_aasist_best.pt` exists at `experiments/M09-detector-recovery/m09_a_aasist_best.pt` (SHA256: `1b9bf59addd8f98422b295fe7ab613b48b0e93eaaadceb0d8ffb5a81d53bda39`).

---

## 2. Launching the Application

### Option A: One-Command Launcher (Recommended for SIH Demo)
Run:
```bash
python scripts/start_sih_demo.py
```
This script automatically:
1. Verifies checkpoint SHA256 integrity.
2. Checks `NVIDIA_API_KEY` status in `.env`.
3. Starts Uvicorn server on `http://localhost:8000/`.
4. Opens your default web browser to the cockpit UI.

### Option B: Manual Server Command
```bash
python -m uvicorn backend.app.main:app --host 0.0.0.0 --port 8000 --reload
```

---

## 3. Mobile Device Access & Local HTTPS Proxy

To access the Web Cockpit from a mobile phone (iOS Safari or Android Chrome) on the same local Wi-Fi network:

1. **Find Local IP Address**:
   - Windows: `ipconfig` (e.g. `192.168.1.105`).
2. **Access via Mobile Browser**:
   - Open `http://192.168.1.105:8000/` on your mobile browser.
3. **Microphone Permission / HTTPS Note**:
   - Mobile browsers restrict microphone access on unencrypted HTTP connections except for `localhost`.
   - For remote mobile testing, use a lightweight SSL tunnel such as ngrok:
     ```bash
     ngrok http 8000
     ```
   - Open the resulting `https://xxxx.ngrok-free.app` URL on your phone for full microphone access.
