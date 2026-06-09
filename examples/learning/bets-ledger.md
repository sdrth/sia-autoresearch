# Bets Ledger (example)

Reference shape for `<lab>/notes/bets-ledger.md` (spec sections 10 and 10A). Every bet
written in any run's `bets.md` gets one line here with a concrete check-by date; the
meta-review resolves due bets and recomputes the hit rate. This is the only mechanism
that measures whether the lab's *judgment* — not its writing — is good.

| Run | Bet (one line) | Logged | Check by | Status | Resolution note |
|-----|----------------|--------|----------|--------|-----------------|
| product-bets-001 | Defining one activation metric lifts trial-to-paid | 2026-05-30 | 2026-07-01 | right | first-value event defined; trial-to-paid +4pts by 2026-06-28 |
| product-bets-001 | Replacing blank first login with templates raises week-1 retention | 2026-05-30 | 2026-07-15 | open | — |
| product-bets-001 | Intent-forked onboarding beats the generic tour | 2026-05-30 | 2026-08-01 | open | — |
| strategy-002 | Waiting on verticalization until activation is fixed costs nothing measurable | 2026-06-01 | 2026-09-01 | open | — |

## Hit rate

- Resolved: 1 right / 0 wrong / 0 unresolved — hit rate: 1.00 (n=1; too small to lean on)

Notes:

- Resolutions are logged as `bet_resolved` events in `runs/autoresearch.jsonl`.
- A poor hit rate on high-scored runs is a scoring instruction (spec section 8):
  score lower until the lab's judgment earns the range back.
- `unresolved` is honest when the check-by date passed but the world gave no answer —
  do not force a verdict.
