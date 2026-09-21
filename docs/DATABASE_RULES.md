# Database Rules

## Safety rules

- Use additive migrations unless a destructive change is separately and explicitly approved.
- Preserve legacy rows and legacy calculations. New architecture must route around them rather than rewrite them.
- Treat accounting ledgers as append-only. Correct mistakes with explicit reversal or compensating entries that preserve the audit trail.
- Never destructively delete financial history.
- Archive units with dependent costs, funding, settlements, allocations, fees, or other accounting records. Do not hard-delete dependency-bearing units.
- Never drop financial tables or use cascading deletion to erase accounting history.
- Do not use silent fallback when required accounting metadata cannot be persisted. Return a visible failure and keep the previous state intact.
- Do not run experiments, temporary financial transactions, cleanup deletes, or unreviewed diagnostics against production.
- Before an approved production data mutation, snapshot or back up every affected row and record the selection criteria.
- Do not infer a cash transaction from an economic record. Cash attribution requires evidence and must be recorded once.

## Ledger corrections

Posted or historical accounting entries remain immutable. A correction must reference the original record where the schema supports it, reverse its accounting effect, and append the corrected entry. Settlement workflows must use their explicit reversal mechanism. Never update an old amount solely to force a total to match.

## Known Legacy Risks — Do Not Fix Opportunistically

The following existing findings are known technical and security debt:

- Financial `ON DELETE CASCADE` relationships exist.
- Some verification scripts create and delete financial test records.
- Some financial tables expose direct delete grants.
- Older investor-oriented views and functions remain in SQL history or the current schema.
- Some financial tables have permissive anonymous write policies.
- `SETUP.md` contains legacy "start fresh" guidance.

Their existence does not authorize changing them during unrelated work. Each requires a separately inspected, planned, verified, and approved hardening migration or change. Do not execute legacy verification scripts against production merely to test them.

## Schema change workflow

Every schema change follows this sequence:

`inspect -> plan -> migration -> review -> verify -> approve -> apply -> verify again`

### Inspect

Map current readers, writers, tables, views, functions, triggers, constraints, row-level security, grants, and dependent application code. Record the current data shape and identify Legacy Accounting dependencies.

### Plan

Define ownership, invariants, forward path, rollback path, compatibility window, and verification queries. Separate additive expansion from any later data migration or destructive contraction.

### Migration

Create an ordered, versioned migration in the repository. Prefer nullable additions, new tables, new views, and compatible function changes. Make reruns idempotent where safe. Do not apply the migration at this stage.

### Review and verify

Review the full diff. Verify syntax and behavior in an isolated non-production environment with representative, disposable test data. Verification must expose partial failure and confirm both old and new paths during any mixed-version window.

### Approve and apply

Production application requires explicit approval for the reviewed migration. Snapshot affected production rows first, apply only the approved migration, capture results, and stop on error.

### Verify again

Verify row counts, constraints, ledger invariants, derived views, permissions, application reads and writes, and the affected financial totals. Preserve the rollback materials. Do not perform a destructive contract step as part of an approved additive rollout unless it received separate approval.
