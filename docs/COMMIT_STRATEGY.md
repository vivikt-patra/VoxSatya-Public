# Commit Strategy for VoxSatya (`docs/COMMIT_STRATEGY.md`)

This document defines the semantic commit message conventions, branching strategy, and versioning scheme for the **VoxSatya** repository. Every contributor to Team Elite Warriors must follow these standards.

---

## 📝 Semantic Commit Message Format

Every commit must use the following format:

```text
<type>(<scope>): <short summary in present tense, lowercase, ≤72 chars>

<optional detailed body explaining the what and why, not the how>
```

### Commit Types:

| Type | Usage |
|:---|:---|
| `feat` | New features, modules, or capabilities |
| `fix` | Bug fixes, runtime error resolutions |
| `refactor` | Code restructuring without behavior change |
| `style` | UI theme changes, layout alignment, visual updates |
| `docs` | Documentation additions or corrections |
| `test` | Test cases added or updated |
| `chore` | Build configuration, dependency updates, CI/CD |
| `security` | Vulnerability patching, secret rotation, encryption updates |
| `perf` | Performance optimizations, latency improvements |

### Scope Examples for VoxSatya:

| Scope | Area |
|:---|:---|
| `android` | Android mobile Kotlin source |
| `ui` | Jetpack Compose user interface |
| `telecom` | InCallService, dialer role, telephony |
| `edge` | ONNX Runtime Mobile, EdgeInferenceEngine |
| `dsp` | VocalStressEngine, audio signal processing |
| `ledger` | SHA-256 chained forensic Room SQLite |
| `backend` | FastAPI server, WebSocket streaming |
| `docs` | README, STRUCTURE.md, technical reports |
| `checklists` | Portfolio publishing gates |

### Well-Formed Commit Examples:
```text
feat(edge): add INT8 quantized AASIST-L inference with ONNX Runtime Mobile
fix(telecom): separate ACTION_DIAL intent filter for Android 14+ RoleManager compliance
style(ui): convert dark theme to white light theme using VoxTheme semantic tokens
security(backend): add CORSMiddleware and enforce zero raw audio disk retention
docs(readme): add team attribution, tech stack, and SIH 2026 architecture diagram
checklists(portfolio): add public/private safety gates, release and secret scan guides
```

---

## 🌿 Branching Strategy

| Branch | Purpose |
|:---|:---|
| `main` | Primary stable, public-facing production branch |
| `feat/<name>` | Feature development branches (merge to main via PR) |
| `fix/<name>` | Bug fix branches |
| `release/vX.Y.Z` | Release preparation branches |

---

## 🔖 Semantic Versioning

VoxSatya follows **Semantic Versioning 2.0.0** (`MAJOR.MINOR.PATCH`):

- `MAJOR`: Breaking architectural changes (e.g., migrating to a fundamentally different edge runtime).
- `MINOR`: New features with backward compatibility (e.g., adding a new forensic export format).
- `PATCH`: Backward-compatible bug fixes and minor UI improvements.

Current production version: **`v2.4.0`**
