# AI Agent Operating Rules

These instructions apply to every AI coding agent working in this repository. Production financial data is high risk. Do not modify production financial data without explicit approval for the specific mutation.

## Repository identity

Canonical repository:

- GitHub: `prakorso/perkasamotors`
- Local canonical checkout: `/Users/panji/Vibes Coding/perkasamotors`

Before modifying files, verify:

- `git rev-parse --show-toplevel`
- `git branch --show-current`
- `git status`

If the working directory resolves to another copy of Perkasa Motors, stop and report it instead of editing that copy. Do not assume another folder named `perkasamotors` is the canonical repository.

## Required workflow

1. Never work directly on `main`. Create or use a task-specific feature or chore branch.
2. Inspect the relevant application paths, database objects, documentation, and current Git state before editing.
3. Explain the plan before implementation.
4. Make the smallest coherent change in the layer that owns the behavior.
5. Show `git diff` after implementation and review it for unintended changes.
6. Run focused verification before any commit.
7. Stop before push, merge, deployment, migration application, or any production database write unless the user explicitly approves that exact next step.

Do not commit unless the user requests it. Never interpret approval to inspect, plan, edit locally, or prepare a migration as approval to apply it in production.

## Source-of-truth priority

- Actual production financial data must never be mutated merely to match documentation.
- Versioned migrations and the current implemented schema describe current technical behavior.
- `AGENTS.md` and repository documentation describe intended safety and business rules.
- If implementation contradicts documented safety rules, report the contradiction and plan a migration or hardening change.
- Do not silently fix production or rewrite history.
- User-approved business decisions override outdated documentation, but update the documentation through Git afterward.

## Financial and data safety

- Never drop financial tables.
- Never use cascading deletion to remove accounting history.
- Never rewrite legacy accounting. Preserve it as recorded and route new accounting through the new model.
- Never fabricate financial transactions, adjustments, dates, payers, or cash movements to force reconciliation.
- Never hardcode financial totals in application logic or reporting. Store valid source events and derive totals from the responsible ledger or view.
- Never treat an economic record automatically as a bank cash transaction. A cost, funding record, settlement classification, liability, or reserve classification is not proof of a bank movement.
- All database schema changes require ordered, versioned migrations. Do not make ad hoc production schema edits.
- Prefer additive, backward-compatible migrations. Make destructive or contract steps separate and require explicit approval.
- Preserve existing rows. Accounting ledgers use append-only entries or explicit reversals; do not edit history in place merely to change an outcome.
- Dependency-bearing units must be archived, not hard-deleted.
- Do not silently continue when required metadata cannot be persisted. Fail visibly and leave the prior financial state intact.
- Do not experiment in production. Before any approved production data mutation, capture a backup or snapshot of affected rows, apply the smallest reviewed change, and verify again.

## Accounting boundaries

Keep these concepts separate in storage, calculations, APIs, UI labels, and reports:

- Founder/Participant Capital
- Perkasa Capital/Equity
- Debt Liability
- Debt Cash Received
- Financing Fee
- Partner Funding
- Unit Cost
- Capital Deployed
- Sale Proceeds
- Realized Profit
- Reserve
- Bank Cash
- Derived Liquidity
- Legacy Accounting

In particular, Unit Cost and Unit Funding are different measures. Purchase cost and funding can describe the same acquisition event and must not become two cash outflows. Sale proceeds and capital return classify one sale cash event and must not become two inflows. Reserve is a restricted classification within cash unless evidence shows an actual transfer to another account.

## Database change sequence

For any proposed schema change, map readers, writers, data shape, compatibility window, and ownership first. Define the forward and rollback paths. Use this sequence:

`inspect -> plan -> versioned migration -> review -> verify -> approve -> apply -> verify again`

Stop at the stage the user approved. Keep mixed-version operation safe during rollout, make retries idempotent where practical, and make partial failure visible.
