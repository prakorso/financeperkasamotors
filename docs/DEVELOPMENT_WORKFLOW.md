# Development Workflow

## Git path

All changes follow this path:

`main -> feature/chore branch -> inspect -> plan -> implement locally -> review diff -> verify -> commit -> push -> PR/merge -> deploy`

`main` is conceptually protected even when GitHub branch protection is not configured. Never edit or commit directly on `main`. Create a focused `feature/...` or `chore/...` branch first.

Each arrow is a control point, not automatic authorization for the next action. A request to prepare or implement a change locally does not authorize a commit, push, merge, deployment, migration application, or production database write.

## Four gates

### Gate 1: Inspect

- Confirm the current branch and working tree.
- Read repository instructions and relevant documentation.
- Trace current readers, writers, data ownership, and invariants.
- For financial changes, identify Legacy Accounting impact and whether an economic event has separate cash evidence.
- Report contradictions or unsafe existing behavior before relying on it.

Exit condition: the current behavior and affected surfaces are understood.

### Gate 2: Plan

- Explain the intended change before implementation.
- Define scope, acceptance criteria, verification, and material risks.
- For schema work, define a versioned migration, forward path, rollback path, compatibility window, and production verification.
- Identify any approval needed for sensitive, destructive, or production actions.

Exit condition: a reviewable plan preserves accounting and data invariants.

### Gate 3: Local implementation

- Implement only on the task branch.
- Keep application logic, database changes, and documentation within the approved scope.
- Review the complete `git diff` and show it to the user.
- Run the smallest sufficient local verification before commit.
- Commit only when requested.

Exit condition: the local change passes verification and the diff contains only intended files.

### Gate 4: Production

Push, PR/merge, deploy, migration application, and production data writes are separate production actions. Stop before each unapproved action. For an approved database mutation, first snapshot affected rows; afterward, verify the production state and financial invariants again.

Exit condition: the specifically approved production action is complete and its result is verified.

## No direct request-to-production path

No request may skip directly to production. Urgency does not remove inspection, planning, local review, verification, backup requirements, or explicit production approval.
