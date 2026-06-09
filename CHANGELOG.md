# CHANGELOG

## v2.0 — 2026-06-09

Spec **1.2.0**. Everything below is **file contracts, not code** — no runtime, server, telemetry, or vector store added. New manifest fields are optional in the schema so older runs stay valid.

### Usability & agent discipline (1.1.0 foundation)

- **`start/QUICKLOOP.md` (new):** condensed loop reference for agents with tight context; full spec wins on conflict.
- **Spec versioning:** `start/AUTORESEARCH-START.md` carries a version header; bootstrap records `specVersion` in `config/setup.md`.
- **`start/schemas/` (new):** JSON Schemas for `manifest.json` and `score.json` — agents self-validate by reading them; no tooling required.
- **Scoring calibration anchors:** spec §8 points at shipped example runs (0.76 keep vs 0.38 discard) so agents calibrate against artifacts, not adjectives.
- **Language mirroring (Q11):** full reader profile at setup — role, proficiency, goals, KPIs; `config/styles/audience.md` as a living vocabulary contract; pre-report language check in the loop.
- **Interactive setup interview:** structured question tools when available; inline viewer surfaces latest run, agent picks, proof of work.
- **`AGENTS.md` + spec:** compact chat indexes, deeper research per run, Q10c static viewer (no server by default).

### Adversarial pass (required)

- **`adversarial.md` (new required artifact):** after the winning thesis emerges and before scoring, the agent attacks its own thesis — strongest counter-arguments, ≥ 2 genuine counter-evidence search branches, steelman of the opposing view (spec §§7–9). If the attack lands, revise the memo before bets and scoring.
- **Hard downgrade trigger (no override):** no adversarial pass, or a pass with zero real counter-evidence searched, caps `overall` below 0.70 (spec §8).
- Manifest gains `adversarial` object (`performed`, `counterQueries`, `thesisRevised`, `summary`).

### Compounding memory

- **`notes/claims.json` + `start/schemas/claims.schema.json`:** claims ledger — what the lab believes, with confidence, status lifecycle, staleness dates, and source-registry evidence ids. Runs read relevant claims as priors (step 2) and write deltas after the verdict (step 16); counts mirrored in manifest `claimsDelta`.
- **`notes/hypothesis-index.md`:** tested-and-discarded theses with revisit triggers — dead theses are not re-researched.
- **`notes/playbooks/<kind>.md`:** per-kind tactics that worked; meta-review-only updates, ~20-line cap.

### Source discovery + registry

- **`config/sources/sources.json` + `start/schemas/sources.schema.json`:** living source registry replacing one-shot `sources/seeds.json` — typed, tiered, weighted entries with `addedBy: owner | agent`. **Registry-first search** before the open web; registry grows every run. `config/sources/README.md` for 30-second owner edits.

### Self-learning loops (spec §10A)

- **Principle:** every signal must change a file the next run is required to read.
- **Owner feedback:** `owner_feedback` jsonl events + one-question-per-session ritual; `audience.md` extended into a learned preference profile.
- **Bet ledger with resolution:** `notes/bets-ledger.md` — every bet logged with a check-by date; meta-reviews resolve due bets, log `bet_resolved` events; running hit rate is a scoring input.
- **Weight update rule** for query seeds and sources: keep +0.05, discard −0.05, `acted_on` +0.10, ignored-keep −0.05; retire below 0.70, cap 1.50.
- **Write-back meta-review:** every 5th run is a checklist with required file mutations — a meta-review that changes no files is incomplete; logs a `meta_review` jsonl event.
- **Calibration self-audit:** every 10 runs, blind re-score 2 past runs; local calibration anchors join the shipped examples.

### Viewer tiers (UI stays optional)

- **Spec §15:** Tier 0 (none), Tier 1 static HTML (default), Tier 2 local app **only on explicit owner opt-in via Q10d** (default: no app, no server).
- **`start/viewer/APP-GUIDE.template.md` + spec §15C:** opt-in local app blueprint — read-mostly, filesystem-backed; sole write path is intent files under `<lab>/queue/`.
- **`start/viewer/BUILD-GUIDE.template.md`:** updated for 1.2.0 contracts (`adversarial.md`, claims ledger, bet hit-rate, source registry, new jsonl events).

### Examples

- **`examples/runs/2026-05-30-product-bets-001/`** (0.76 keep) — good normal run; `adversarial.md` where thesis survives without gaining score.
- **`examples/runs/2026-06-01-strategy-002/`** (0.72 keep) — memory compounding on run 001, counter-evidence branches, adversarial pass sharpens one bet.
- **`examples/runs/2026-06-03-competitive-intel-003/`** (0.38 discard) — calibration anchor for conservative scoring; adversarial pass reinforces the discard; `claimsDelta` all zero.
- All three example runs upgraded to **spec 1.2.0** (`adversarial`, `claimsDelta`, `adversarial.md`) so calibration anchors stay valid under the new contracts.
- **`examples/learning/` (new):** bets-ledger and write-back meta-review reference snippets.
- `examples/README.md`, `README.md`, `AGENTS.md`, `start/QUICKLOOP.md` synced.

---

## v1 — 2026-05-31

Initial public release of [sia-autoresearch](https://github.com/sdrth/sia-autoresearch).

- Spec-first research lab for founders and builders — one agent spec drives structured loops
- Standalone lab or embedded in any repo (`start/` + `<lab>/` path rules)
- Run kinds: product-bets, strategy, growth, competitive-intel, founder-writing, code-research
- Proof-of-work artifacts, conservative scoring, optional viewer (demos in `examples/`)
- Uses only the user's agent and default web search — no telemetry
- Inspired by [Karpathy's autoresearch](https://github.com/karpathy/autoresearch)
