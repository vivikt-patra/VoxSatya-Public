# VoxSatya Team Workflow

This workflow lets each team member develop VoxSatya independently while keeping the shared `main` branch stable and reviewable.

## First-time setup

1. Accept the GitHub collaborator invitation.
2. Install Git and sign in to GitHub on the development computer.
3. Clone the private repository:

```powershell
git clone https://github.com/vivikt-patra/VoxSatya.git
cd VoxSatya
```

4. Follow the setup and test commands in the root [`README.md`](../README.md).
5. Create a local `.env` from `.env.example` and enter only personal development credentials. Never share or commit `.env`.

## Start each piece of work

Update the local copy and create a focused branch. Replace the example branch name with a short description of the task.

```powershell
git switch main
git pull --ff-only origin main
git switch -c feature/short-task-name
```

Use prefixes such as `feature/`, `fix/`, `test/`, or `docs/`.

## Save and publish work

Review changed files before committing:

```powershell
git status
git diff
```

Run the relevant tests, then commit and push the branch:

```powershell
git add <reviewed-files>
git commit -m "feat: describe the completed change"
git push -u origin feature/short-task-name
```

On GitHub, open a pull request from the new branch into `main`. Describe:

- what changed and why;
- how it was tested;
- screenshots for interface changes;
- limitations or unfinished follow-up work.

Another team member should review the pull request before it is merged.

## Coordination rules

- Do not commit directly to `main` for normal feature work.
- Pull the latest `main` before beginning a new task.
- Keep one logical task per branch and pull request.
- Do not force-push shared branches or rewrite published history.
- Do not commit recordings, datasets, model weights, databases, exported evidence, dependency folders, build caches, or credentials.
- Record incomplete or proposed work honestly; a roadmap item is not an implemented capability.
- Preserve the acoustic-detector authority, privacy boundaries, and human-verification safeguards documented in the README.

## Access scope

Collaborator permission is granted only on `vivikt-patra/VoxSatya`. It does not grant access to other repositories owned by `vivikt-patra`.
