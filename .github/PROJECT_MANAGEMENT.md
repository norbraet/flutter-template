# Project management playbook

The canonical delivery model comes from `docs/roadmap.md`. GitHub configuration is stored as code in this directory and synchronized with `scripts/bootstrap-github.ps1`.

## One-time setup

From the repository root, authenticate a maintainer account and run:

```powershell
gh auth login -h github.com
./scripts/bootstrap-github.ps1
```

Use `-WhatIf` to preview API-backed label and milestone changes. The script is idempotent: it updates matching labels and milestones, reuses a project with the configured title, links it to the repository, and adds missing project fields.

After bootstrapping, use `.github/project.yml` to create the saved Roadmap, Delivery board, Priorities, and Backlog views and enable the listed built-in project workflows. GitHub currently manages these view/workflow details most reliably in the project UI.

## Issue readiness

An issue may move to Ready when it has one type, one priority, at least one area, an owner or explicit pickup path, a milestone (or Future backlog), testable acceptance criteria, known dependencies, and no unresolved product-boundary question.

## Operating cadence

- Weekly triage: classify Inbox, request missing information, remove duplicates, and reject out-of-scope work.
- Weekly delivery review: inspect blocked and in-progress work, limit work in progress, and update risks.
- Milestone review: assess exit criteria from `docs/roadmap.md`, not ticket count alone.
- Release review: verify accessibility, offline/recovery, privacy, migrations, attribution, and platform readiness.

## Estimation and priority

Effort is relative: XS is a few hours, S is roughly a day, M is several days, L should usually be split, and XL requires decomposition before Ready. Priority reflects impact and urgency: Critical is reserved for security, data loss, release blockers, or unusable core flows.
