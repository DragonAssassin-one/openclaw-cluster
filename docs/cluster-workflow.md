# OpenClaw Cluster Workflow (4-Instance GitHub Flow)

This repo is used by a local 4-instance OpenClaw "cluster" (Alpha/Beta/Gamma/Delta)
running from separate working copies on the same machine. The instances do not
share state/memory automatically; GitHub is the collaboration layer.

The goal is repeatable, low-confusion collaboration even when a single person
operates all roles via multiple local instances.

## Roles (Suggested)

- Alpha (Implementer): makes code changes and keeps diffs small.
- Beta (Reviewer): reviews diffs, finds risks/regressions, asks for tests/evidence.
- Gamma (Designer): produces design/plan and task breakdown.
- Delta (Operator): validates runtime, startup/stability, logs, rollback steps.

## Branch Naming

Use short kebab-case topics. Examples: `tool-policy`, `fleet-scripts`, `oauth-fix`.

- `design/<topic>`: design/plan only (docs, diagrams, decisions).
- `feat/<topic>`: feature work (code + tests).
- `fix/<topic>`: bug fix (minimal change).
- `ops/<topic>`: runbooks, deployment/verification notes, operational changes.
- `chore/<topic>`: maintenance (deps, lint, formatting, CI tweaks).
- `hotfix/<topic>`: urgent fix intended to merge quickly.

Rules:

- One topic per branch.
- Avoid long-lived branches. Keep PRs small and merge frequently.
- Do not commit local runtime state: `state/`, `workspace/`, `.fleet/`, `.serena/`.

## PR Rules (Lightweight)

- Every non-trivial change should be a PR (even if you're solo).
- PR description must include:
  - What changed / what did NOT change (scope boundary)
  - How to verify (commands + expected output)
  - Failure recovery / rollback steps
- If a design exists, link the `design/<topic>` PR or a design doc.

Suggested review gates:

- Beta: approves after reading diff + checking risks/tests.
- Delta: approves after runtime verification (start/stop, logs clean, ports ok).

## Solo "4-Instance" Loop (Recommended)

1. Gamma: create `design/<topic>` with a short plan and acceptance criteria.
2. Alpha: create `feat/<topic>` from `origin/main` and implement.
3. Beta: fetch `feat/<topic>`, review, request fixes, or push `fix/<topic>` and let Alpha cherry-pick.
4. Delta: fetch `feat/<topic>`, run runtime verification, add ops notes (or comment in PR).
5. Merge to `main` (prefer squash merge unless you need full history).

## Keeping Up With Upstream (Optional)

If you keep the official repo as `upstream`:

1. `git fetch upstream`
2. `git checkout main`
3. `git merge upstream/main` (or rebase if you prefer)
4. `git push origin main`

Resolve conflicts in Alpha, then re-run verification in Delta.

