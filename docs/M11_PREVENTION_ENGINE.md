# M11 Action Safety & Prevention Engine Documentation

## 1. Executive Summary
The **Prevention Engine** (`audio_engine/prevention/engine.py`) translates raw neural detector scores, temporal risk states, and DSP quality indicators into explicit, human-understandable safety recommendations.

> [!IMPORTANT]
> **Human-In-The-Loop Safety Mandate**:
> The Prevention Engine enforces `autonomous_action_permitted = False` across all policy decisions. The system **never** automatically blocks bank accounts, terminates real phone calls, transfers money, or posts public accusations. All sensitive decisions require human verification.

---

## 2. Policy Matrix

| Risk State | Prevention Level | Action Headline | Safety Policy & Guidance |
| :--- | :---: | :--- | :--- |
| **GENUINE** | `NONE` | Voice Authenticity Normal | Audio exhibits natural vocal tract constraints. Continue conversation normally while remembering AI detection is probabilistic. |
| **UNCERTAIN** | `CAUTION` | Inconclusive Voice Evidence | Audio confidence is ambiguous or signal quality is low. Do not make high-stakes financial decisions based on this audio. Obtain a clearer sample. |
| **HIGH_RISK** | `CRITICAL_ALERT` | Potential Cloned/Replayed Voice Detected | Synthetic voice clone or acoustic replay detected with high confidence. **DO NOT** transfer money, share OTPs, or trust urgent financial requests. Initiate an independent callback. |
| **LOW_SNR / CLIPPED** | `CAUTION` | Low Audio Quality — Exercise Caution | Ambient noise or audio clipping detected. Request speaker to move to a quieter location. |
| **ERROR / DISCONNECT** | `SYSTEM_NOTICE` | Detection Unavailable | Real-time monitoring is disconnected or unmonitored. Do not present caller as verified. |

---

## 3. Decoupled Architecture
- **Independent Operation**: The Prevention Engine runs purely on deterministic rule policies derived from Platt-calibrated probability ranges and DSP quality metadata.
- **LLM Independence**: Nemotron LLM assistant does **not** evaluate or modify the prevention policy; it only translates the resulting structured policy into conversational explanation text if requested.
