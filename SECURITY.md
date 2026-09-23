# Security Policy

## Supported version

Security fixes are applied to the latest version on the repository's default branch.

## Reporting a vulnerability

Please do not disclose suspected vulnerabilities through a public GitHub issue. Contact the repository owner privately through their GitHub profile and include:

- the affected component and version or commit;
- clear reproduction steps;
- the likely impact;
- any suggested mitigation.

Do not include live credentials, private audio, personal information, or production data in a report. Use synthetic or redacted evidence wherever possible.

## Repository safety

- Secrets belong only in an untracked `.env` file or a deployment secret store.
- `.env.example` contains placeholders only.
- Raw voice recordings, datasets, model weights, local databases, generated evidence packages, logs, and dependency folders are excluded from version control.
- Detection results are decision support; high-impact action remains subject to human verification.
- Demo bearer tokens work only when `DEPLOYMENT_PROFILE=DEMO_LOCAL` is selected explicitly; the default profile rejects them.

## Scope boundary

This repository is a research and competition prototype. Its documented lab and benchmark results do not constitute certification for unsupervised production use.
