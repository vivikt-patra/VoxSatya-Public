# M11 Known System Limitations & Disclosure

## 1. Overview
In accordance with scientific and engineering rigour, this document explicitly lists the verified limitations of the current SIH 2026 Voice Cloning Impersonation Defense prototype.

---

## 2. Documented Technical Limitations

### 2.1 Digital Direct A17 Neural Vocoder Attack
- **Observed Behavior**: In direct digital file evaluation (memory-to-memory without acoustic playback), compressed neural vocoder attack **A17** exhibits smooth spectral representations that bypass raw filterbanks (0.93% direct digital detection rate in M09).
- **Acoustic Mitigation**: Physical acoustic re-recording through speakers and air restores detection to **100.00%** in M10 due to acoustic room impulse response (RIR) signatures.
- **Future Scope**: Codec-augmented multi-condition fine-tuning in future releases.

### 2.2 Additive Ambient Noise ($<10\text{ dB SNR}$)
- **Observed Behavior**: Heavy ambient noise (e.g. loud street noise or heavy machinery) degrades signal-to-noise ratio, causing the quality guard to map genuine speech to `UNCERTAIN` or `LOW_SNR`.
- **System Safeguard**: The system transitions to `UNCERTAIN` with low-quality notices rather than silently issuing false fraud accusations.

### 2.3 Mobile Operating System Call Interception Boundaries
- **Observed Behavior**: Modern mobile operating systems (iOS and Android) sandbox third-party apps from capturing protected telephony calls (Cellular, WhatsApp, Telegram).
- **Deployment Model**: The prototype operates as a Mobile-Accessible Web/PWA application during active user sessions with explicit microphone permission.

---

## 3. Product Claims Disclaimer
1. The system **does not** claim 100% universal accuracy across every unseen voice clone algorithm.
2. The system **does not** claim automatic background call interception across third-party mobile apps without OS framework permissions.
3. Machine learning outputs are probabilistic; all sensitive decisions mandate **Human-In-The-Loop** independent verification.
