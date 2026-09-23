# M10 Live Cockpit Demo Runbook (SIH Judge Demonstration)

## 1. Overview & Setup
This runbook provides the step-by-step procedure for demonstrating real-time voice cloning detection, temporal risk scoring, prevention warning alerts, and Nemotron assistant interaction to SIH judges.

### Prerequisites
- Python 3.14 environment with project dependencies.
- Frozen M09 model checkpoint at `experiments/M09-detector-recovery/m09_a_aasist_best.pt`.
- Web browser with microphone access permission.

---

## 2. Step-by-Step Demo Flow

### Step 1: Launch Backend Server
Run the Uvicorn FastAPI server:
```bash
python -m uvicorn backend.app.main:app --reload --port 8000
```
Open browser to: `http://localhost:8000/`

### Step 2: Open Real-Time Cockpit
- Navigate to the **Real-Time Stream Cockpit** section.
- Observe the initial state: `MICROPHONE OFF`, audio level bar empty, status badge inactive.

### Step 3: Demonstrate Live Genuine Speech
1. Click **START LISTENING**.
2. Grant browser microphone access when prompted.
3. Speak normally into the microphone for 5-10 seconds:
   *"Hello, this is a live genuine voice sample for the SIH 2026 presentation."*
4. **Observe Cockpit Output**:
   - Audio input meter responds dynamically to live speech.
   - Status badge transitions to `LISTENING` -> `GENUINE` (Green).
   - Real-Time Factor (RTF) displays `~250x`, Latency displays `< 10ms`.
   - Prevention Alert Banner remains hidden.

### Step 4: Demonstrate Replay / Synthetic Impersonation Attack
1. Maintain active stream.
2. Play a synthetic voice clone sample or replayed speech recording through a secondary device (phone or speaker) positioned near the microphone.
3. **Observe Cockpit Output**:
   - Rolling buffer accumulates acoustic replay signatures.
   - Detector score rises; status badge transitions to `HIGH_RISK` (Red).
   - **Prevention Alert Banner** appears immediately:
     > 🚨 **CRITICAL PREVENTION WARNING**: Potential cloned/replayed voice detected! Do not trust urgent financial or identity requests based solely on this voice. Verify through an independent channel.

### Step 5: Ask Downstream Nemotron Assistant
1. In the **Nemotron 3.5 Security Assistant** card, click **Ask Assistant for Guidance**.
2. Nemotron analyzes structured evidence and generates human-readable safety advice:
   - Identifies suspicious acoustic indicators.
   - Recommends callback verification protocols.

### Step 6: Stop Listening & Verify Privacy
1. Click **STOP LISTENING**.
2. Cockpit returns to `MICROPHONE OFF`.
3. Session memory buffer is immediately cleared (`PRIVACY_BY_DEFAULT`).
