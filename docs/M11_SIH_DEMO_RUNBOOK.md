# SIH 2026 Golden Demonstration Runbook

## 1. Overview
This runbook provides the step-by-step procedure for demonstrating the SIH 2026 Voice Cloning Impersonation Defense prototype to Smart India Hackathon judges.

---

## 2. One-Command System Launch
Run the launcher script from terminal:
```bash
python scripts/start_sih_demo.py
```
This automatically verifies frozen AASIST checkpoint SHA256 integrity, launches the FastAPI server on `http://localhost:8000/`, and opens the browser cockpit UI.

---

## 3. Judge Demonstration Walkthrough (3-Minute Flow)

```
+------------------+     +-------------------+     +------------------+     +-------------------+
| 1. Open Cockpit  | --> | 2. Live Genuine   | --> | 3. Play Clone    | --> | 4. Ask Nemotron   |
| (Microphone OFF) |     | (Green: GENUINE)  |     | (Red: HIGH_RISK) |     | (Safety Guidance) |
+------------------+     +-------------------+     +------------------+     +-------------------+
```

### Step 1: Open Cockpit (10 Seconds)
- Show the **Live Real-Time Stream Cockpit** header.
- Highlight the initial status: `MICROPHONE OFF`.
- Point out the Privacy Statement: *"Audio analyzed in memory; zero raw audio stored by default."*

### Step 2: Test Live Genuine Speech (45 Seconds)
- Click **START LIVE STREAMING**. Grant browser microphone permission.
- Speak live: *"Hello judges, this is a live genuine voice sample for the SIH 2026 evaluation."*
- **Observe Output**:
  - Live Audio Level Meter responds dynamically to voice.
  - Status badge displays **GENUINE** (Green).
  - Processing Latency displays `< 10 ms` (Real-Time Factor: `~260x`).
  - Prevention Banner remains inactive.

### Step 3: Demonstrate Synthetic / Replayed Impersonation Attack (45 Seconds)
- Maintain active streaming session.
- Play a synthetic voice sample or replayed recording from a secondary phone near the microphone.
- **Observe Output**:
  - Rolling buffer detects acoustic replay / TTS artifacts.
  - Status badge immediately transitions to **HIGH_RISK** (Red).
  - **Critical Prevention Alert Banner** appears with bold action guidance:
    > 🚨 **CRITICAL WARNING**: Potential cloned/replayed voice detected! DO NOT perform financial transactions or share OTPs based on this call.

### Step 4: Downstream AI Assistant Guidance (45 Seconds)
- Click **Ask Assistant for Guidance** in the Nemotron Security Assistant card.
- Show Nemotron 3.5 Lightning response:
  - Explains spoof findings in plain language.
  - Recommends independent callback verification protocols.
- *Optional Resilience Demo*: Unset `NVIDIA_API_KEY` to demonstrate that detection and warnings continue working seamlessly via rule-based fallback.

### Step 5: Stop Streaming & Verify Privacy (15 Seconds)
- Click **STOP LISTENING**.
- Show that session buffer is immediately purged (`StreamSession.clear()`).
