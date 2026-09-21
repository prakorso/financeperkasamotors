# Financial Architecture

## Purpose and source boundaries

Perkasa Motors uses two accounting paths. Legacy Accounting preserves historical records and calculations. The new operating model records direct unit costs, Perkasa and Partner funding, settlements, reserve allocations, founder capital, debt, and derived liquidity as distinct concepts. New work must not retroactively rewrite the legacy path.

Database ledgers and settlement snapshots are the accounting sources of truth. Views derive totals from those sources. A derived view is not independent evidence that a bank transaction occurred.

## Verified facts and current checkpoints

The amounts in this document are verified historical or economic facts and accounting checkpoints from the current documented state. They are not permanent hardcoded totals. Future legitimate transactions may change aggregate balances. The permanent requirement is to preserve accounting semantics and historical facts, not to freeze every aggregate forever.

Historical and economic facts must remain historically preserved:

- Founder contributions of Rp40M from Panji and Rp20M from Pandu, dated 2026-08-01.
- Debt principal of Rp74.7M, net disbursement of Rp73.2M, and financing fee of Rp1.5M for the documented debt event.
- Unit #62 historical settlement economics.
- Unit #75 deployment date and pre-baseline treatment.
- Legacy records.

Current checkpoint values may legitimately change through future transactions:

- Total restricted reserve of Rp2.15M.
- Actual operational bank balance of Rp1.95M.
- Any aggregate derived liquidity or account balance.

Never hardcode these checkpoint values into application logic. None of these figures may be replaced with inferred or fabricated values merely to make another report reconcile.

### Capital

| Classification | Amount | Date or status |
|---|---:|---|
| Legacy founder capital | Rp757.5M | Legacy Accounting |
| New founder capital: Panji | Rp40M | Economic date 2026-08-01 |
| New founder capital: Pandu | Rp20M | Economic date 2026-08-01 |
| New founder capital: total | Rp60M | Economic date 2026-08-01 |

The new founder contributions belong to the Founder/Participant Capital ledger. They are separate from cumulative legacy founder balances, Perkasa unit funding, Partner Funding, debt, revenue, and liquidity.

### Debt

| Classification | Amount |
|---|---:|
| Gross Debt Liability | Rp74.7M |
| Net Debt Cash Received | Rp73.2M |
| Financing Fee | Rp1.5M |

Gross Debt Liability represents the principal obligation. Net Debt Cash Received is the amount received as cash. The Financing Fee accounts for the difference and remains separate from the liability.

### Restricted reserve

| Classification | Amount |
|---|---:|
| Total restricted reserve | Rp2.15M |
| Unit #62 override | Rp450K |

Reserve is a restriction or allocation within cash unless funds were demonstrably transferred elsewhere. It must not be counted as cash a second time or treated as a separate bank balance without transfer evidence.

### Unit baselines

| Unit | Classification | Amount or date |
|---|---|---:|
| #62 | Base Unit Cost | Rp84.55M |
| #62 | Perkasa Funding | Rp84.2M |
| #62 | Funding Gap | Rp350K |
| #62 | Selling Price | Rp89M |
| #62 | Realized Profit | Rp4.45M |
| #62 | Capital Return | Rp84.2M |
| #66 | Base Unit Cost | Rp10.25M |
| #66 | Perkasa Funding | Rp5M |
| #66 | Partner Funding | Rp5M |
| #66 | Funding Gap | Rp250K |
| #75 | Base Unit Cost | Rp78.1M |
| #75 | Perkasa Funding | Rp74.5M |
| #75 | Deployment date | 2026-08-17 |
| #75 | Liquidity treatment | Pre-baseline relative to 2026-08-18 |
| #42 | Legacy active capital allocations | Rp77M |
| #45 | Legacy identity | Yamaha R15 V3 Black |
| #45 | `kas_bisnis` | Rp200K |

Unit #42 is a Legacy unit and must not be hard-deleted while dependencies exist. Archive it if it must leave active operating lists.

### Liquidity and bank balance

- D10 `actual_cash` is a derived, provisional liquidity position. It is not reconciled Bank Cash.
- The actual bank balance known operationally is Rp1.95M.
- The unresolved bank variance is Rp200K.
- Do not fabricate a balancing transaction. Resolve the variance only with evidence of the underlying bank movement or a separately approved accounting treatment.

## Required accounting distinctions

### Unit Cost and Unit Funding

Unit Cost measures direct economic cost attributable to a vehicle. Unit Funding measures the capital sources assigned to finance the unit. Their difference is the Funding Gap. A cost may exist economically even when payer, payment status, payment date, or bank evidence is unknown.

Purchase cost and funding may represent the same acquisition event from different accounting perspectives. Cash reporting must attribute the actual payment once; it must not count the cost entry and the funding entry as two cash outflows.

### Sale proceeds, capital return, and profit

A sale creates one sale cash event. Sale Proceeds describe the gross or net receipt from that event. Capital Return and Realized Profit classify components of that same receipt. They are not additional inflows.

For Unit #62, the recorded settlement economics are:

- Base Unit Cost: Rp84.55M
- Explicit Perkasa Funding: Rp84.2M
- Funding Gap: Rp350K
- Selling Price: Rp89M
- True Unit Profit: Rp4.45M
- Capital Return snapshot: Rp84.2M

The Rp89M Selling Price minus the Rp84.55M Base Unit Cost yields the Rp4.45M True Unit Profit. The Rp350K difference between Base Unit Cost and explicit Perkasa Funding consists of direct unit cost that is not represented as additional explicit unit funding. Therefore, Capital Return remains Rp84.2M under the recorded settlement snapshot. Do not invent a cash source for the Rp350K unless payment evidence exists.

### Capital, partner support, and debt

- Founder/Participant Capital records founder contributions, returns, and approved adjustments.
- Perkasa Capital/Equity is the company's own capital classification.
- Partner Funding supports a particular unit or collaboration. It is not Founder Capital.
- Debt Liability is the obligation owed. Debt Cash Received is the amount of cash actually disbursed. Financing Fee is separate from both.

### Reserve, bank cash, and derived liquidity

Reserve restricts the use of cash but does not by itself move cash. Bank Cash is established from bank evidence and reconciliation. Derived Liquidity is a model output built from a baseline and recorded flows. D10 liquidity therefore supports operating analysis but does not constitute bank reconciliation.

## Economic records and cash evidence

Economic costs can exist without confirmed cash-payment evidence. Optional payment attribution records who paid, whether it was paid, and when; absence of this metadata must remain unknown. It must not default silently to company cash, Perkasa cash, Partner cash, or a bank movement.
