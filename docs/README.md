# Documentation Directory

System architecture specs, research logs, threat models, and evaluation protocol documentation.

## Sections
* Architecture: Core product signal pipeline and prevention workflow.
* Threat Model: Voice cloning impersonation attack vectors (deepfakes, replay, voice changers, adversarial perturbations).
* Benchmarks: Anti-spoofing EER, min t-DCF, calibration metrics.

## Android implementation authority

The Android-led SIH26104 application is isolated at [../voxsatya-android/README.md](../voxsatya-android/README.md). It contains the future Android source tree plus the PRD, architecture, engineering approach, technology stack, chronological roadmap, API contract, test strategy and physical-device setup guide.

For the complete handoff package, including the supplied market/architecture/UI research and its technical evaluation, use [VOXSATYA_MASTER_SPEC.md](../voxsatya-android/team-handoff/VOXSATYA_MASTER_SPEC.md). The concise [TEAM_HANDOFF_BLUEPRINT.md](../voxsatya-android/TEAM_HANDOFF_BLUEPRINT.md) remains useful for workshop discussion.

Its [research and implementation plan](../voxsatya-android/docs/RESEARCH_AND_IMPLEMENTATION_PLAN.md) contains the detailed research record, while the [readiness audit](../voxsatya-android/docs/READINESS_AUDIT.md) records verified current behavior and limitations.
