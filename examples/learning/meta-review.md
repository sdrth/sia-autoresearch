# Write-Back Meta-Review (example)

Reference shape for the dated `## Meta-review` section in `<lab>/notes/autoresearch.md`
(spec section 10). The defining property: **a meta-review that changes no files is
incomplete.** This example shows the checklist executed after runs 001–005 of the
fictional B2B SaaS lab from `examples/runs/`.

---

## Meta-review — 2026-06-15 (runs 001–005)

### Files changed by this review

- `notes/bets-ledger.md` — resolved 1 due bet (`right`); hit rate 1.00 (n=1)
- `config/queries/seeds.json` — `activation metric benchmarks SaaS` +0.05 (keep) +0.10 (acted_on) → 1.15; retired `onboarding tool market whitespace` (0.65 after two discard hits)
- `config/sources/sources.json` — `src-002` (Lenny) +0.10 → 1.15, timesUseful 2; `src-007` (vendor blog roundup, agent-added) retired below 0.70
- `notes/claims.json` — re-verified `clm-001` (activation > pricing as constraint), confidence 0.7 → 0.8, reviewBy pushed to 2026-09-15
- `notes/playbooks/product-bets.md` — added: "lead with one measurable first-value event; owner acts on bets framed against trial-to-paid, ignores funnel-shape framing" (cap respected: 7/20 lines)
- `notes/autoresearch.md` — local calibration anchors set: best keep = product-bets-001 (0.76), clearest discard = competitive-intel-003 (0.38)
- `runs/autoresearch.jsonl` — 1 `bet_resolved` event + this `meta_review` event

### Observations (5–8 bullets)

- Repeating theme across keeps: activation/first-value framing wins; pricing and
  feature-parity branches keep dying — candidate follow-up: instrumenting the
  first-value event (code-research kind).
- Owner feedback: `acted_on` for 001 (shipped the activation metric), `ignored` for
  the strategy memo's third option — preference profile updated in `audience.md`
  (bets-first, short verdicts).
- Scoring drift check: recent overalls 0.76 / 0.72 / 0.38 — distribution healthy
  against shipped anchors; no recalibration note needed this cycle.
- Dead seed retired (see files above) — whitespace scans need a sharper question
  before that topic earns another loop.
- One thing the next 5 runs do differently: every product-bets run must check
  `clm-001` against fresh product data instead of re-arguing it from market sources.

```json
{"timestamp":"2026-06-15T10:30:00Z","event":"meta_review","runsCovered":5,"filesChanged":["notes/bets-ledger.md","config/queries/seeds.json","config/sources/sources.json","notes/claims.json","notes/playbooks/product-bets.md","notes/autoresearch.md"],"betHitRate":1.0}
```
