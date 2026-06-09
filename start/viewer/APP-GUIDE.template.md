<!--
TEMPLATE — do not implement from this file directly, and NEVER without explicit
owner opt-in (Q10d = yes, or the owner asks for an app later in plain words).

The default sia-autoresearch lab is filesystem + static HTML viewers only — no app,
no server, no npm. This template exists so that when an owner explicitly asks for a
richer local app, the build follows the lab's trust model instead of improvising.

When the owner opts in, the agent customizes this template using the repo snapshot,
enabled kinds, Q10 actions, and audience vocabulary from `{{lab}}/config/setup.md`,
then writes the result to `{{lab}}/dashboard/APP-GUIDE.md`.

Replace every {{...}} placeholder. Delete this comment block in the output.
-->

# {{repo-name}} — Autoresearch Local App Viewer: Build Guide (Tier 2)

**Opt-in record:** {{date + where the owner explicitly asked for an app — quote them}}
**Lab root:** `{{lab}}/`
**Enabled kinds:** {{comma-separated list from setup}}
**Audience:** {{role + proficiency from config/styles/audience.md — the app speaks their language}}

---

## What this is — and is not

A **local-first, read-mostly workbench** for the human distillation job: triage runs,
inspect proof of work, watch the lab's calibration, and queue intents for the agent.

- **It is not** a research runner. The agent loop stays the only writer of research.
- **It is not** a service. No backend, no database, no auth, no telemetry, no network
  calls needed to render runs. Local source links may open on click.
- **It is removable without loss.** All truth lives in the lab files. Deleting the
  app loses nothing; the static viewer (Tier 1) and raw `runs/` (Tier 0) still work.

## Trust boundaries (hard rules)

1. **Reads** come straight from the filesystem: `{{lab}}/runs/`, `{{lab}}/notes/`,
   `{{lab}}/config/`. No copies of truth, no sync layer, no database.
2. **The only write path** is intent files under `{{lab}}/queue/` (shape below). The
   app never writes run artifacts, scores, verdicts, claims, registry entries, or
   anything under `runs/` or `notes/`.
3. **No new schemas.** The app consumes the contracts the agent loop already writes
   (spec sections 8, 10, and `start/schemas/`).
4. **Local only.** Runs on the owner's machine with one documented command. No
   deploy target, no accounts.

## Stack

- {{Suggested: Next.js (App Router) + shadcn/ui + Tailwind, or the stack this repo
  already uses — inspect before adding anything}}
- **Pin exact dependency versions** in `package.json` at scaffold time (no `^`/`~`
  ranges) so the app builds identically months later.
- Prefer `pnpm`. Keep the app self-contained in `{{lab}}/dashboard/app/` so the lab
  root stays clean.
- Data loading: server components / loaders reading via `fs` — never an HTTP API in
  front of the lab files.

---

## Screens

Build in this order; each screen earns the next.

### 1. Triage queue (home)

The owner's job is distillation — optimize for it.

- **Data:** `runs/autoresearch.jsonl` + `runs/[run-id]/manifest.json`
- Runs awaiting human reaction (no `owner_feedback` event yet), sorted by score;
  each row: thesis one-liner, kind, score, verdict, top bet, "why not higher" snippet
- Quick intent buttons per row: {{Q10 actions — e.g. promote / annotate / discard}}
  → each writes one intent file to `{{lab}}/queue/` (shape below)

### 2. Run detail — proof of work first-class

- **Data:** everything under `runs/[run-id]/`
- Tabs: Hypotheses (graveyard prominent) · **Adversarial** (attack vectors, counter
  queries, outcome) · Notes (sources with `src-NNN` registry links) · Memo · Bets ·
  Verdict · Scores
- Header: hypothesis counts, `claimsDelta`, adversarial outcome badge
  (survived / revised / killed)

### 3. Calibration view

Score drift is silent — make it visible.

- **Data:** manifests + `notes/autoresearch.md` (local anchors, recalibration notes)
- Score distribution over time vs. the shipped anchors (0.76 keep / 0.38 discard)
  and this lab's local anchors; flag when recent runs cluster above 0.75

### 4. Claims ledger browser

- **Data:** `notes/claims.json` (schema: `start/schemas/claims.schema.json`)
- What the lab believes now: claims by status and confidence, formed-by run links,
  stale claims (past `reviewBy`) flagged; contradicted claims kept visible — knowing
  what stopped being true is signal

### 5. Bet hit-rate tracker

- **Data:** `notes/bets-ledger.md` + `bet_resolved` jsonl events
- Open bets with check-by dates, resolved right/wrong, running hit rate over time —
  the lab's verified track record

### 6. Source registry view + edit requests

- **Data (read):** `config/sources/sources.json` (schema: `start/schemas/sources.schema.json`)
- Browse the registry: sources by tier, topic, `addedBy`, and weight.
- The registry is owner-editable **by contract**, but the app still does not write it
  directly — that would break trust boundary rule 2 and make the app a second writer
  of lab truth. Instead, "add a source / re-tier / edit topics" emits an `edit-source`
  **intent** to `{{lab}}/queue/` (shape below); the agent applies it to `sources.json`
  on its next pass. The app may validate the proposed entry against the schema before
  writing the intent, and never proposes a `weight` (agent-managed, section 10A rule).
- Owners who want a truly direct edit still have one: open `sources.json` in any text
  editor. The app stays read-mostly so there is exactly one programmatic writer of lab
  files — the agent.

---

## Intent files (`{{lab}}/queue/`)

The app's only write path. One JSON file per intent; the agent consumes and archives
them on its next pass (move to `queue/done/`, never silently delete).

```json
{
  "createdAt": "2026-06-10T12:00:00Z",
  "intent": "promote | annotate | request-run | set-feedback | edit-source",
  "runId": "2026-06-09-product-bets-004",
  "payload": {
    "note": "free text from the owner",
    "reaction": "acted_on | ignored | disagreed | revisit",
    "source": {
      "id": "src-NNN (or null to let the agent assign)",
      "name": "...",
      "url": "...",
      "type": "primary | data | paper | docs | news | community | blog",
      "qualityTier": 1,
      "topics": ["..."]
    }
  }
}
```

Filename: `{{lab}}/queue/YYYY-MM-DDTHHmmss-<intent>.json`. Carry only the fields an
intent needs (`runId` is omitted for `edit-source`; `source` is omitted for the
others). The agent applies intents on its next pass:

- `set-feedback` → an `owner_feedback` jsonl event (the app never appends to
  `autoresearch.jsonl` itself)
- `edit-source` → a validated entry in `config/sources/sources.json` with
  `addedBy: "owner"` (the app never writes the registry itself)
- `promote` / `annotate` / `request-run` → the repo-local workflow each maps to

In every case the app's sole filesystem mutation is the intent file under `queue/`.

---

## Verification

- [ ] Cold start with zero runs: every screen renders an empty state, no errors
- [ ] `pnpm dev` (or the one documented command) is the only step needed
- [ ] Airplane mode: all screens render fully offline
- [ ] Writing an intent file is the only filesystem mutation the app ever makes
      (verify with a file watcher during a click-through)
- [ ] Delete the app folder → lab unaffected; static viewer still works
- [ ] Owner vocabulary from `config/styles/audience.md` used in all labels

## Out of scope (deliberately)

- Running research, scoring, or editing run artifacts from the UI
- Hosted deploys, auth, multi-user sync, databases, telemetry
- Writing `notes/`, `runs/`, or `config/` (including `sources.json`) directly — all
  changes go through `queue/` intents the agent applies, or the owner's own text editor
- Replacing the agent's judgment — the app shows; the human decides; the agent acts
