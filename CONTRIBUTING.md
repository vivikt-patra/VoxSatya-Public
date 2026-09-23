# Contributing to VoxSatya

VoxSatya is an active team project. Changes should remain reviewable, evidence-backed, and safe for a public development repository.

## Development workflow

1. Create a focused branch from the current development baseline.
2. Keep each change small enough to review and describe its user or security impact.
3. Add or update tests for behavior changes.
4. Run the backend tests, frontend type-check, and production build that apply to the change.
5. Open a pull request describing what changed, how it was verified, and any remaining limitation.

The practical GitHub commands and repository-specific collaboration process are documented in [`docs/TEAM_WORKFLOW.md`](docs/TEAM_WORKFLOW.md).

## Non-negotiable safeguards

- Never commit `.env` files, credentials, private recordings, personal information, local databases, model weights, generated evidence archives, dependency folders, or build caches.
- Never weaken the rule that the acoustic detector is the authority for voice-authenticity classification.
- Never enable autonomous high-impact action from a model verdict; preserve human verification.
- Never claim field validation, production readiness, language coverage, or benchmark performance without reproducible evidence.
- Keep demo-only behavior explicitly isolated behind a deliberate local-development configuration.

## Commit guidance

Use descriptive commit messages such as:

```text
feat(streaming): add bounded reconnect handling
fix(auth): reject expired operator sessions
test(detector): cover noisy uncertain verdicts
docs(roadmap): record mobile packaging limitations
```

## Reporting security concerns

Follow [`SECURITY.md`](SECURITY.md). Do not disclose vulnerabilities or live secrets in public issues.
