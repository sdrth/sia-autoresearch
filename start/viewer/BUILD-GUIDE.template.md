<!--
TEMPLATE — do not implement from this file directly.

When Q10b = `guided` or `both`, the autoresearch agent customizes this template
using the repo snapshot, enabled kinds, Q10 actions, and Q10b choice from
`{{lab}}/config/setup.md`, then writes the result to
`{{lab}}/dashboard/BUILD-GUIDE.md`.

Companion doc: `{{lab}}/dashboard/SPEC.md` (what to build — panels, vocabulary,
actions). This guide covers how to build and verify it.

Replace every {{...}} placeholder. Use `<lab>/` paths (not hardcoded `autoresearch/`
unless embedded mode). Reference `examples/` for behavior inspiration — do not copy
demos verbatim unless the owner asks.

Rules for the output:
- Explain WHAT to build, WHY it helps this repo, and FILE CONTRACTS to honor.
- Do NOT include copy-pasteable component code, framework boilerplate, or styling.
- Do NOT pick a visual design — behavior, files, and workflow only.
- Delete this comment block in the output.
-->

# {{repo-name}} — Autoresearch Viewer: Build Guide

Implementation guide for the autoresearch viewer in this repo. Read **`{{lab}}/dashboard/SPEC.md`** first for panels, vocabulary, and actions — then use this doc to scaffold, wire data, and verify.

**Lab root:** `{{lab}}/`
**Viewer:** static HTML at `{{lab}}/dashboard/` (`file://`; Q10c: `{{static-simple | static-interactive}}`)
**Q10b choice:** `{{guided | both}}`
**Enabled kinds:** {{comma-separated list from setup}}

---

## Why this exists

{{1–2 sentences: what this repo is, who uses it, and why a richer viewer helps. Tie to the owner's Q1 objective from `config/objective.md`.}}

{{If Q10b = `both`: one sentence on how this guide extends the inline `dashboard/index.html` the agent already maintains — richer stack, same file contracts, can replace inline later.}}

The viewer is **a read-mostly UI over markdown + JSON files** the agent loop already writes. No backend, no database, no autoresearch API. File reads only; the UI makes runs easier to scan, compare, and act on.

---

## Relationship to SPEC.md

| Doc | Owns |
|-----|------|
| `SPEC.md` | Repo snapshot, enabled kinds, panel list, action labels, owner vocabulary, "done looks like" |
| `BUILD-GUIDE.md` (this file) | File contracts, parsing notes, build order, stack hints, verification |

If `SPEC.md` and this guide disagree, **`SPEC.md` wins** on product behavior; regenerate this guide when the spec changes materially.

---

## What you are building

Build the **smallest viewer** that helps the owner review runs and coordinate the next agent pass. Default surfaces (customize names in `SPEC.md`; keep behavior):

### Panel: {{Run list — owner vocabulary}}
- **Purpose:** Browse all runs with kind, title, score, verdict, and date at a glance.
- **Data:** `{{lab}}/runs/autoresearch.jsonl`, `{{lab}}/runs/[run-id]/manifest.json`
- **Behaviors:** Sort by date (newest first default); link each row to run detail.
- **Empty state:** {{e.g. "No runs yet — complete setup and run loop 001."}}
- **Why it matters here:** {{tie to repo type}}

### Panel: {{Run detail — owner vocabulary}}
- **Purpose:** Read one run's evidence, outputs, and verdict in one place.
- **Data:** `{{lab}}/runs/[run-id]/` — see [Run folder files](#run-folder-files) below.
- **Behaviors:** Tab or section per artifact, including an **Adversarial** tab
  (`adversarial.md` — how the thesis was attacked and what survived); show
  proof-of-work counts from `manifest.json` or `verdict.md`, plus a header badge for
  the adversarial outcome (`adversarial.thesisRevised`) and the `claimsDelta` summary.
- **Empty state:** Prompt to pick a run from the list.
- **Why it matters here:** {{which kinds/outputs matter most in this repo}}

### Panel: {{Agent picks — owner vocabulary}}
- **Purpose:** Surface high-signal runs worth acting on now.
- **Data:** Same as run list; filter where `verdict` is `send_for_review` or `keep`, **or** `scores.overall` ≥ 0.80 in `manifest.json`.
- **Empty state:** {{e.g. "No picks yet — scores ≥ 0.80 or send_for_review runs appear here."}}
- **Why it matters here:** {{what "pick" means for this owner — roadmap, publishing, etc.}}

### Panel: {{Living memory — owner vocabulary}}
- **Purpose:** Show rolling lab context without opening markdown manually.
- **Data:** `{{lab}}/notes/autoresearch.md` — especially `## Recent Runs`, `## Top Queries`, `## Review Queue Snapshot` if present.
- **Empty state:** {{e.g. "Living memory not started — first run will populate notes/autoresearch.md."}}
- **Why it matters here:** {{how the owner uses memory across loops}}

### Panel: {{Activity log — owner vocabulary}}
- **Purpose:** Timeline of setup, run creation, verdict changes, and review feedback.
- **Data:** `{{lab}}/runs/autoresearch.jsonl` (primary); optionally tail `notes/autoresearch.md` for human notes.
- **Behaviors:** Render `event` types (`setup_completed`, `run_created`, `verdict_updated`, etc.) in chronological order.
- **Empty state:** {{e.g. "No activity yet."}}
- **Why it matters here:** {{whether audit trail matters for this repo}}

### Optional panels — 1.2.0 learning surfaces

Include these when the owner cares about how the lab compounds (recommend at least
the bet hit-rate panel — it is the lab's track record). Each reads a file the agent
already maintains; render the empty state if the file is absent.

### Panel: {{Claims ledger — owner vocabulary}}
- **Purpose:** Show what the lab currently believes and how confident it is.
- **Data:** `{{lab}}/notes/claims.json` (schema: `start/schemas/claims.schema.json`).
- **Behaviors:** Group by `status` (`active` / `revised` / `contradicted` / `expired`);
  flag claims past `reviewBy` as stale; link `sourceRun` and `evidence` (`src-NNN`).
- **Empty state:** {{e.g. "No claims yet — runs add them as the lab forms beliefs."}}

### Panel: {{Bet hit-rate — owner vocabulary}}
- **Purpose:** Track whether past bets actually came true — the lab's judgment, measured.
- **Data:** `{{lab}}/notes/bets-ledger.md` + `bet_resolved` events in `autoresearch.jsonl`.
- **Behaviors:** Open bets with check-by dates; resolved right/wrong/unresolved; running hit rate.
- **Empty state:** {{e.g. "No bets logged yet."}}

### Panel: {{Source registry — owner vocabulary}}
- **Purpose:** Browse the trusted sources steering research.
- **Data:** `{{lab}}/config/sources/sources.json` (schema: `start/schemas/sources.schema.json`).
- **Behaviors:** Sources by `qualityTier`, topic, `addedBy` (`owner` / `agent`), and weight.
  Read-only here; edits go through the action surface as requests, never direct writes.
- **Empty state:** {{e.g. "Registry empty — seed it at setup or add sources by hand."}}

{{Add 0–2 repo-specific panels from SPEC.md using the same bullet structure.}}

---

## Actions

Actions are **requests to the agent loop**, not buttons that mutate run files directly. The viewer records or surfaces the request; the next agent pass executes it.

{{If the repo has no established request channel: state "Action surface is informational only — owner triggers actions in chat until a channel is defined."}}

{{For each action from Q10 in setup:}}

### Action: {{label}}
- **Where it appears:** {{panel / run row / run detail}}
- **Owner sees:** {{confirmation copy or status after recording}}
- **Records request in:** {{repo-local channel — e.g. `notes/autoresearch.md` section, issue template, chat handoff file}}
- **Request shape:** {{one line — e.g. append markdown bullet with run id + action + timestamp}}
- **Agent picks it up:** {{which loop / kind; what files get written next}}

---

## File contracts

These files are the single source of truth. **Treat them as a schema.** If a file is missing, render the panel's empty state — never crash.

All paths below use `{{lab}}/` as the lab root (`{{lab-path-description — e.g. "repo root in standalone mode" or "autoresearch/ subfolder in embedded mode"}}`).

### Index and memory

| File | Shape | Used for |
|------|-------|----------|
| `{{lab}}/runs/autoresearch.jsonl` | one JSON object per line, append-only | run list, activity log, latest scores |
| `{{lab}}/notes/autoresearch.md` | markdown sections | living memory, recent runs snapshot, local calibration anchors |
| `{{lab}}/notes/claims.json` | `{ claims: [...] }` (schema: `start/schemas/claims.schema.json`) | claims-ledger panel; what the lab believes |
| `{{lab}}/notes/bets-ledger.md` | markdown table + hit rate | bet hit-rate panel; resolved/open bets |
| `{{lab}}/notes/hypothesis-index.md` | markdown table | optional: show discarded theses + revisit triggers |
| `{{lab}}/notes/playbooks/<kind>.md` | markdown | optional: per-kind tactics that worked |
| `{{lab}}/config/sources/sources.json` | `{ sources: [...] }` (schema: `start/schemas/sources.schema.json`) | source-registry panel |
| `{{lab}}/config/setup.md` | markdown + frontmatter | enabled kinds, viewer mode, git prefs (read-only in viewer) |

### `autoresearch.jsonl` events

Each line is JSON. Common `event` values:

| event | Typical fields | UI use |
|-------|----------------|--------|
| `setup_completed` | `timestamp` | activity log start |
| `run_created` | `runId`, `kind`, `title`, `topic`, `verdict`, `overallScore`, `queries`, hypothesis counts | new run row |
| `verdict_updated` | `runId`, `verdict`, optional `overallScore` | update row + activity log |
| `owner_feedback` | `runId`, `reaction` (`acted_on`/`ignored`/`disagreed`/`revisit`), `note` | mark runs reacted-to; activity log |
| `bet_resolved` | `runId`, `bet`, `resolution` (`right`/`wrong`/`unresolved`), `note` | bet hit-rate panel; activity log |
| `meta_review` | `runsCovered`, `filesChanged`, `betHitRate` | activity log; calibration context |

Parse line-by-line; tolerate partial files during an in-progress run. Treat unknown
`event` values as forward-compatible — render them generically, do not crash.

### Run folder files

Per run: `{{lab}}/runs/[run-id]/`

| File | Shape | Notes |
|------|-------|-------|
| `manifest.json` | `{ id, kind, title, topic, verdict, hypothesisCounts, selectedQueries, selectedSources, scores, adversarial, claimsDelta, outputs, specVersion, createdAt, updatedAt }` | **Primary metadata** for list + detail headers. `adversarial` / `claimsDelta` present on spec ≥ 1.2.0 runs; treat as optional |
| `verdict.md` | markdown; `**Status:**` line + `## Proof of work` + `## Why this is not higher` | Parse status from markdown, not YAML frontmatter |
| `score.json` | numeric dimensions 0.00–1.00 + `overall` | optional if `manifest.scores` already loaded |
| `hypotheses.md` | markdown | proof of work — candidates, discarded branches, winning thesis |
| `notes.md` | markdown | sources (with `src-NNN` registry ids) and raw observations |
| `memo.md` | markdown | internal strategy memo |
| `adversarial.md` | markdown | required on spec ≥ 1.2.0 runs — thesis attacked, counter-queries, what survived/revised; render as its own detail tab |
| `bets.md` | markdown | recommendations (product-bets, growth, etc.) |
| `essay.md` | markdown | optional public draft — omit tab if missing |
| `voice-observations.md` | markdown | founder-writing runs only |

**`manifest.adversarial` shape:** `{ performed: bool, counterQueries: string[], thesisRevised: bool, summary: string }`.
**`manifest.claimsDelta` shape:** `{ added, confirmed, revised, contradicted }` (integers).

**Verdict values:** `discard` | `draft` | `keep` | `send_for_review`

**Agent picks rule:** show run if `verdict ∈ {keep, send_for_review}` **or** `scores.overall ≥ 0.80`.

### Action requests (if applicable)

| File | Shape | Notes |
|------|-------|-------|
| `{{repo-local request channel}}` | {{markdown / jsonl / other}} | append-only or clearly scoped section; viewer writes **requests only**, not run artifacts |

---

## Parsing notes (non-prescriptive)

- **JSONL:** read whole file or tail for dev; sort by `timestamp` when building activity log.
- **manifest.json:** prefer over re-parsing markdown for list views (faster, stable).
- **verdict.md:** extract `**Status:**` value; render remaining sections as markdown.
- **Spec version:** branch on `manifest.specVersion` — runs below 1.2.0 lack
  `adversarial` / `claimsDelta` and have no `adversarial.md`; hide those surfaces for
  older runs instead of showing empty ones.
- **claims.json / sources.json:** plain JSON — validate against the shipped schemas;
  on parse failure render the empty state, never crash.
- **Missing files:** hide tabs/sections; never show broken embeds.
- **In-progress runs:** manifest may exist before verdict — show partial state with a "in progress" badge if `verdict` is `draft`.

---

## Reference demos (behavior only)

Use shipped examples for **interaction patterns**, not copy-paste UI:

| Demo | Borrow | Do not copy |
|------|--------|-------------|
| `examples/dashboard.html` | run list + detail tabs, agent picks, proof-of-work surfacing | Notion styling, embedded demo data |
| `examples/reusable-rockets-thesis.html` | single-thesis reading flow for memo/essay-heavy repos | space/rocket content, one-off layout |

Your stack and visual treatment are your choice.

---

## Implementation context

Inspect this repo before adding dependencies.

- **Suggested for this repo:** {{1–2 options — e.g. "Next.js App Router — `apps/web/` already exists" or "Vite + React in `dashboard/` — no frontend yet, keep it light"}}
- **Data loading:** {{e.g. "Server components + `fs`" or "build-time glob import" or "dev server with file watcher"}}
- **Avoid:** {{e.g. "hosted DB, auth, or anything that cannot read `{{lab}}/runs/` locally in one command"}}

**Hard requirements:**
- Runnable locally with **one documented command** (e.g. `pnpm dev` from `dashboard/`).
- Reads run files from `{{lab}}/` via filesystem — Node `fs`, server components, or build-time loaders.
- Works on the owner's machine without sia-autoresearch infrastructure.

---

## Build order

Ship incrementally; verify each step before continuing.

1. **Scaffold + read-only run list.** Parse `autoresearch.jsonl` + `manifest.json`. Confirm empty state.
2. **Run detail.** One run page with tabs for artifacts that exist for enabled kinds: {{e.g. hypotheses, adversarial, memo, bets, verdict}}.
3. **Agent picks panel.** Apply filter rule; link to detail.
4. **Living memory.** Render tail of `notes/autoresearch.md`.
5. **Activity log.** Chronological events from JSONL (include `owner_feedback`, `bet_resolved`, `meta_review`).
6. **Learning panels (1.2.0).** Bet hit-rate first (`bets-ledger.md`), then claims ledger (`claims.json`), then source registry (`sources.json`). Each reads one file; skip if absent.
7. **Actions (one at a time).** Record request → run one agent loop → confirm pickup → next action.
8. **Stop.** No search, auth, sync, or cloud deploy until the owner has used the viewer for a week.

{{If Q10b = `both`: optional step 0 — diff against `dashboard/index.html`; reuse its panel names where possible.}}

---

## Verification

Run these checks before calling the viewer done:

- [ ] **Cold start:** viewer opens with zero runs — all panels show empty states, no console errors.
- [ ] **After loop 001:** {{enabled kind}} run appears in list within {{realistic time, e.g. "one refresh"}} with correct score and verdict.
- [ ] **Agent picks:** a run with overall ≥ 0.80 or `send_for_review` appears in picks; sub-threshold `discard` does not (unless spec says otherwise).
- [ ] **Run detail:** `hypotheses.md` proof-of-work section visible; `adversarial.md` tab shown for 1.2.0 runs and hidden for older ones; missing `essay.md` does not break layout.
- [ ] **Living memory:** `## Recent Runs` matches latest `autoresearch.jsonl` entry.
- [ ] **Learning panels (if built):** bet hit-rate reflects `bets-ledger.md`; claims ledger groups by status and flags stale claims; source registry lists `sources.json` entries — each shows its empty state when the file is absent.
- [ ] **Activity log:** shows `setup_completed` and at least one `run_created`; `owner_feedback` / `bet_resolved` / `meta_review` events render (generically if unrecognized).
- [ ] **Actions:** {{action 1}} request recorded in {{channel}} and picked up by agent on next loop.
- [ ] **Local-only:** no network calls required to render runs (external source links in notes may open on click).

---

## Done looks like

From `SPEC.md` — confirm these owner outcomes:

- [ ] {{owner-specific outcome 1 — e.g. "See new product-bets within 10s of a run finishing"}}
- [ ] {{outcome 2 — tied to enabled kinds}}
- [ ] {{outcome 3 — tied to Q10 actions}}

---

## Out of scope (deliberately)

- Component code, JSX, CSS, or design tokens — implementer chooses.
- A bespoke API server for run data — re-read [File contracts](#file-contracts).
- Auth, multi-user sync, or deployment — local-only by default.
- Writing run artifacts, `notes/`, or `config/sources/sources.json` from the viewer — only action **requests** in the approved channel; the agent applies them.
- New JSON schemas — consume what the agent loop already writes (`manifest`, `score`, `claims`, `sources`).

---

## Maintenance

| Trigger | Action |
|---------|--------|
| New run completes | No guide change; viewer reads fresh files |
| `setup.md` changes kinds, Q10 actions, or vocabulary | Update `SPEC.md`; regenerate this guide if panels or contracts change |
| New run output type added to spec | Add row to [Run folder files](#run-folder-files) and a detail tab in implementation |

The autoresearch agent regenerates this file when the spec changes materially.
