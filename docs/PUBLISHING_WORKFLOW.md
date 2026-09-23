# Publishing Workflow (`docs/PUBLISHING_WORKFLOW.md`)

This document describes the end-to-end workflow from local development through security verification to public GitHub release for the **VoxSatya** platform.

---

## 📋 Publishing Pipeline Overview

```text
[1. Local Development]
        │
        ▼
[2. Pre-Commit Checklist]  ←── checklists/pre_commit_checklist.md
        │  Passes?
        ▼
[3. Automated Secret Scan] ←── scripts/test_track_b_nemotron.py
        │  Zero secrets?
        ▼
[4. Unit Test Gate]
        │  Android + Backend green?
        ▼
[5. Pre-Push Gate]         ←── checklists/pre_push_gate.md
        │  All gates pass?
        ▼
[6. Public Safety Review]  ←── checklists/PUBLIC_RELEASE_CHECKLIST.md
        │  Approved?
        ▼
[7. git push origin main]
        │
        ▼
[8. GitHub Release Tag]    ←── checklists/final_release_checklist.md
```

---

## 🔄 Standard Local → GitHub Sync Command Sequence

```powershell
# 1. Stage all intentional changes
git add -A

# 2. Verify staged content is clean and intentional
git diff --cached --stat

# 3. Run secret audit
.venv\Scripts\python.exe scripts/test_track_b_nemotron.py

# 4. Create semantic commit
git commit -F commit_message.txt    # or use -m "feat(scope): message"

# 5. Push to public origin
git push origin main
```

---

## 🏷️ Creating a Public Release Tag (For Competition Milestones)

When preparing for SIH demonstration or submission:

```bash
# Create annotated version tag
git tag -a v2.4.0 -m "Release v2.4.0: Autonomous on-device edge defense & white theme UI"

# Push tag to GitHub (creates a Release entry)
git push origin v2.4.0
```

Then upload the compiled APK directly to the GitHub Release page as a binary asset (not tracked in git).
