# sia-autoresearch — Portable Start Guide

**Purpose:** One file an agent can follow to stand up and run sia-autoresearch as a
**standalone founder research system** or inside **any repo** for product, growth,
strategy, competitive intel, founder writing, and code research without modifying
existing repo files or docs.

**Mode:** Manual and file-based. No app, CLI, or automation required unless the repo
owner explicitly asks for it later.

---

## 1. What sia-autoresearch Is

sia-autoresearch is an isolated research lab inside a single top-level folder. It is built
for founders and builders who want AI to multiply their research loops without giving
up judgment. Each **run** turns external signals + optional repo context into:

- source notes with links
- an internal memo (dense, actionable)
- concrete bets or recommendations
- an optional public-facing draft (essay, post, changelog note, RFC sketch)
- a score and a keep/discard verdict

Runs accumulate memory so the lab does not rediscover the same ideas every session.
The system expands the number of loops AI can run; the human still distills the signal.

**Hard boundary:** Everything autoresearch creates lives under one folder (default:
`autoresearch/`). Do **not** edit app code, canonical docs, decision files, or task
plans unless the repo owner explicitly promotes an output and asks you to apply it.

Autoresearch can appear in two valid operating modes:

- **Embedded in an existing repo** — `autoresearch/` sits inside another project. The
  rest of that project is read-only context unless the owner explicitly asks for
  promotion or edits outside `autoresearch/`.
- **Standalone clone / standalone lab** — the current repo is itself the research lab.
  There may be no parent product repo to scan. In this mode, treat the repo as a
  self-contained workspace for research artifacts, examples, notes, and config.

---

## 2. Agent Rules (Read First)

When asked to "run autoresearch" or "start autoresearch":

0. **Detect the operating mode and resolve paths.**

   | | **Standalone lab** | **Embedded mode** |
   |---|---|---|
   | **Detect when** | `start/AUTORESEARCH-START.md` exists and there is no parent product/codebase outside the lab | `autoresearch/` is a subfolder inside a larger project with app code, product docs, or canonical files outside it |
   | **Spec file** | `start/AUTORESEARCH-START.md` | `autoresearch/AUTORESEARCH-START.md` |
   | **Lab root `<lab>/`** | repo root (`.`) | `autoresearch/` |
   | **Setup gate** | `config/setup.md` | `autoresearch/config/setup.md` |
   | **Runs** | `runs/` | `autoresearch/runs/` |
   | **Living memory** | `notes/autoresearch.md` | `autoresearch/notes/autoresearch.md` |
   | **Viewer (optional)** | chosen at setup (Q10b) | chosen at setup (Q10b) |

   In **standalone lab** mode (this repo's layout), shipped reference material lives at
   `start/`, `examples/`, and public docs — read them, do not rewrite them without
   explicit approval. Create runtime lab artifacts at repo root: `config/`, `notes/`,
   `runs/`, `publishing/`. A viewer is **not** shipped with the repo; demo viewers live
   in `examples/`. The owner and agent choose viewer style during setup (Q10b).

   In **embedded mode**, create and edit only under `autoresearch/`. Read the
   surrounding repo read-only for context.

   Record the detected mode in `<lab>/config/setup.md` once setup is complete.

1. **Create only inside `<lab>/`.** Never modify files outside `<lab>/` without explicit
   permission. In standalone lab mode, `start/`, `examples/`, and shipped public docs
   are outside `<lab>/` and are read-only unless the owner asks otherwise.
2. **Use context appropriate to the mode.**
   - In **embedded mode**, read the surrounding repo read-only for context (README,
     plans, architecture, recent commits). Optionally copy or symlink reference
     material into `<lab>/internal-mirror/` — never write back to the originals.
   - In **standalone mode**, do not pretend there is a larger repo to scan. Use `start/`,
     `examples/`, existing lab files, owner-provided notes, local documents, and web
     research as your primary context.
3. **Use the agent's default search tools** when the task needs current external
   signals (news, competitors, standards, papers, benchmarks). Do not invent sources
   from memory when live research is needed.
4. **One run = one folder** under `<lab>/runs/`.
5. **Append every run** to `<lab>/runs/autoresearch.jsonl`.
6. **Update living memory** in `<lab>/notes/autoresearch.md` after each run.
7. **Score honestly.** Weak runs are archived, not forced into publishable shape.
8. **No auto-promotion.** Strong outputs stay in the lab until a human moves them into
   the main repo (embedded mode) or explicitly promotes them (standalone mode).
9. **Interview before first loop.** If `<lab>/config/setup.md` is missing or not
   `complete`, run the startup interview (section 3) before any research loop.
   Do not guess priorities from the repo scan alone.
10. **Search before synthesis.** Generate multiple candidate hypotheses, query angles,
    and search branches before committing to a final memo. Do not lock onto the first
    plausible answer.
11. **Show proof of work.** Record how many hypotheses were considered, which search
    queries were actually used, which branches were discarded, and why the winning
    thesis beat the alternatives.
12. **Score with restraint.** A completed run is not automatically a strong run. High
    scores must be earned through non-obvious signal, evidence quality, and clear
    comparative advantage over discarded branches.
13. **Use extra reasoning capacity when available.** If the current agent has access to
    stronger models, subagents, or parallel research helpers, it may use them
    deliberately for bounded subtasks such as parallel search, hypothesis validation,
    or source gathering. The parent agent remains responsible for final judgment,
    synthesis, and scoring.
14. **No bundled network services.** This spec does not ship telemetry, analytics, or
    sia-autoresearch-specific API calls. Use only the agent, tools, and MCP servers
    the owner already runs. Web research uses the agent's **default built-in search**
    — not a custom search backend from this repo.

---

## 2A. Security and trust model

sia-autoresearch is **spec-first and file-based**. It is markdown instructions plus
local artifacts — not a hosted service, CLI daemon, or background agent platform.

### What this repo does not do

- No phone-home, telemetry, analytics, or usage reporting shipped with the spec
- No required API keys, accounts, or third-party services to run loops
- No external calls made *by this repository itself* — it is static documentation and
  examples

### What actually runs

Everything executes inside **the agent environment the user already chose** (Cursor,
Copilot, Claude Code, ChatGPT, etc.) and whatever tools that environment exposes:

- read/write files under `<lab>/`
- read-only repo context
- **web search via the agent's default available search tool** when external signals
  are needed
- optional subagents or stronger models **only if the user has already enabled them**

The spec must not instruct agents to install extra packages, CLIs, SDKs, or SaaS
integrations unless the owner explicitly asks.

### User responsibility

**You are responsible for security.** The owner controls:

- which agent runs the spec and what permissions it has (file access, network, shell)
- which MCP servers, plugins, or subagents are enabled
- what repo content the agent can read (including secrets, credentials, or private data)
- what gets committed vs kept local (Q9)
- whether run outputs are promoted outside `<lab>/`

Review agent permissions, gitignore choices, and run artifacts before sharing a repo or
publishing outputs. Treat web search like any other agent network access — queries and
results flow through your agent provider's tooling, not through sia-autoresearch.

### Agent hard rules (security)

- Do not add telemetry, tracking pixels, analytics scripts, or background sync
- Do not call sia-autoresearch-specific endpoints — none exist
- Do not exfiltrate repo contents to external services unless the owner explicitly
  requests that workflow
- Do not install dependencies or run installers as part of default bootstrap
- Prefer citing sources in `notes.md` with links; do not scrape or store credentials

---

## 3. Startup Interview (Required Before First Loop)

Autoresearch starts with a short **mode check + context scan + owner interview**. The
agent first determines whether it is embedded in an existing repo or operating as a
standalone lab. Then it proposes what kinds of research fit this environment; the owner
confirms or overrides. Only then write config and run loop 001.

**Gate:** Do not start a research loop until `<lab>/config/setup.md` has `status: complete`.

### 3A. Detect first start

Run the interview when **any** of these is true:

- `<lab>/config/setup.md` is missing
- `<lab>/config/setup.md` has `status: pending` or no status field
- **Embedded mode only:** `autoresearch/` does not exist yet (bootstrap required first)

**Skip the interview** when:

- `<lab>/config/setup.md` has `status: complete` and the user asks for a specific loop
- The user explicitly says to reuse existing setup and run N

### 3B. Repo scan (read-only, no edits outside `<lab>/`)

First determine which scan path applies:

- **Embedded mode:** scan enough of the surrounding repo to infer what the project is
  and what research would help.
- **Standalone mode:** scan enough of the autoresearch repo itself plus owner-provided
  local materials to infer what the owner wants the lab to optimize for.

#### Embedded mode scan

Typical files:

| Signal | Where to look |
|--------|----------------|
| What the project is | Root `README.md`, `package.json`, `pubspec.yaml`, `Cargo.toml`, etc. |
| Product direction | `docs/plans/`, `docs/product/`, `PRD`, `ROADMAP`, `STATUS.md` |
| Architecture | `ARCHITECTURE.md`, `docs/architecture/`, top-level app folders |
| Decisions / constraints | `DECISIONS.md`, `docs/decisions/`, ADRs |
| Growth / GTM | `docs/marketing/`, landing copy, analytics specs |
| Engineering health | CI config, `TODO`, issue labels, test layout, monorepo structure |
| Recent focus | Last 5–10 commit messages, open PR titles |

Produce a **repo snapshot** (5–10 bullets):

- project type (app, library, monorepo, infra, docs, research, etc.)
- primary user or customer
- current stage (idea, build, launch, scale, maintenance)
- dominant stack or surface
- obvious open questions or tensions visible from docs/code
- 2–3 research angles that seem naturally relevant (**recommendations only**)

Do **not** mirror the whole repo. Optional: copy 3–5 high-signal files into
`internal-mirror/` after the interview if runs will need them.

#### Standalone mode scan

When there is no surrounding product repo, build the snapshot from:

| Signal | Where to look |
|--------|----------------|
| What the owner wants from the lab | Root `README.md`, examples, notes, prompt from the owner |
| Current research surfaces | `examples/`, `notes/`, `publishing/`, local documents the owner points you to |
| Existing memory or seeds | `config/queries/`, `config/sources/`, `notes/autoresearch.md` |
| Future connection points | Owner-provided notes, bookmarks, or external workflows |

Produce a **standalone lab snapshot** (5–10 bullets):

- primary owner / operator
- main research jobs this lab should serve
- current stage (blank setup, first runs, repeated use, publishing support)
- dominant input types (notes, links, repo context, web research, drafts)
- obvious gaps in the current lab setup
- 2–3 research angles that seem naturally relevant (**recommendations only**)

In standalone mode, `internal-mirror/` is optional and often unnecessary.

### 3C. Interview the owner (in chat)

Present the repo snapshot, then ask the questions below. **Pre-fill a recommended
answer for every question** based on the scan — same pattern as a decision sheet.
The owner confirms, edits, or replaces each recommendation.

Keep it to **6–8 questions** per session. Add 1–2 repo-type follow-ups from section
3D when relevant.

#### Core questions (always ask)

Before Q1, state which mode you detected:

- `Mode: embedded in an existing repo`
- `Mode: standalone autoresearch lab`

**Q1. Lab objective**  
What should autoresearch optimize for in *this* repo over the next few weeks?

*Agent recommendation:* [one sentence from repo snapshot]

**Q2. Run kinds (priority order)**  
Which run kinds do you want, in priority order? Pick 2–4.

Options: `product-bets`, `strategy`, `code-research`, `growth`, `competitive-intel`,
`founder-writing`

*Agent recommendation:* [ordered list + one-line why each fits this repo]

**Q3. Output mode**  
Mostly internal memos and bets, or also public-facing drafts (essays, posts, RFC
sketches)?

*Agent recommendation:* [internal-only | internal + selective publishing]

**Q4. Focus topics**  
What domains should loops explore first? (3–6 topics)

*Agent recommendation:* [bullets derived from repo + market context]

**Q5. Avoid**  
What should loops **not** waste time on?

*Agent recommendation:* [e.g. out-of-scope features, settled decisions, hype topics]

**Q6. Loop cadence**  
Single loops on demand, or periodic batches (e.g. 5 or 20 parallel subagent loops)?

*Agent recommendation:* [single | small batch | large batch + when]

**Q7. Review handoff**  
When a run is strong, how should it reach you?

*Agent recommendation:* [publishing queue file | summary in chat | `best-of/` only]

**Q8. First loop**  
What is the first sharp question loop 001 should tackle?

*Agent recommendation:* [one specific topic + suggested kind]

**Q9. Git preferences**  
By default, research runs and drafts are local-only (not committed). Which of the following should be committed to the repo?

*Defaults — local-only (not committed):* `runs/`, `state/`, `internal-mirror/`, `publishing/queue/`, `publishing/approved/`, `publishing/archived/`  
*Defaults — committed:* `config/`, `notes/`, `publishing/templates/`  
*Defaults — local-only:* viewer files (if any), unless the owner opts in below

Common overrides:
- "Commit best-of/" — commit curated winners
- "Commit all runs" — full research trail in git
- "Commit viewer" — commit `<lab>/dashboard/` if a viewer was built

*Agent recommendation:* [confirm defaults | list any overrides based on repo context]

**Q10. Web viewer actions** *(only if Q10b ≠ `skip`)*  
If the owner wants a viewer, what should it make easy to see or request? (Select or describe 2–4 actions.)

Common actions by repo type:

| Repo type | Suggested actions |
|-----------|------------------|
| Product / app | "Send bet to roadmap", "Draft this as a spec", "Archive" |
| Founder / writing | "Draft as essay", "Add to publishing queue", "Keep for reference" |
| B2B / growth | "Test this channel bet", "Send to growth backlog", "Discard" |
| Engineering / OSS | "Open as RFC", "Link to issue tracker", "Archive" |
| Strategy / research | "Promote to decision doc", "Flag for review", "Discard" |

The viewer will also show: agent picks (score ≥ 0.80), all runs with scores and verdicts, living memory snapshot, and an activity log of setup, run updates, verdict changes, and review feedback.

*Agent recommendation:* [2–4 actions tailored to repo scan + enabled kinds]

**Q10b. Viewer style**
How should runs be browsed? The agent recommends one option based on repo context and
owner preference. **There is no committed dashboard in the sia-autoresearch repo** —
use `examples/` (especially `reusable-rockets-thesis.html`) as reference demos only.

Options:
- `skip` — no viewer; read runs directly from `runs/` and chat reports (valid default for minimal setup)
- `inline` — agent builds a self-contained `<lab>/dashboard/index.html` and updates it after runs (section 15A). Design is the agent's choice.
- `guided` — agent writes `<lab>/dashboard/SPEC.md` + `<lab>/dashboard/BUILD-GUIDE.md` describing what the viewer should support; a follow-up agent or owner implements it in whatever stack they use (section 15B)
- `both` — inline viewer for immediate use plus spec + guide for a richer viewer later

*Agent recommendation:* [pick one and say why — prefer `skip` when the owner will read runs in chat or markdown; prefer `inline` or `guided` when browsing history matters; cite `examples/` when showing what a viewer could look like]

1. Post the repo snapshot + all questions with recommendations in **one message**.
2. Ask the owner to reply inline (e.g. `Q2: …`, `Q4: …`) or say "accept all
   recommendations."
3. Do **not** create run folders or query seeds until answers are captured.
4. If the owner is unavailable, write `config/setup.md` with `status: pending`,
   store scan + recommendations, and stop — do not run loop 001.

### 3D. Repo-type follow-ups (pick 0–2)

Add when the scan clearly fits:

| Repo shape | Extra question |
|------------|----------------|
| Consumer product app | Which user job or wedge should product bets stress-test first? |
| B2B / infra / SDK | Architecture and adoption bets, or competitive positioning first? |
| Monorepo | Per-surface research (mobile, web, backend) or cross-cutting themes only? |
| Library / OSS | API ergonomics and migration paths, or ecosystem/competitive intel? |
| Pre-product / idea repo | Strategy and market validation before code-research? |
| Mature maintenance repo | Tech-debt/refactor bets or growth/expansion bets? |

### 3E. Capture answers → `config/setup.md`

After the owner responds, write `config/setup.md`:

```markdown
---
status: complete
completedAt: 2026-05-31T12:00:00Z
interviewedBy: agent-id
---

# Autoresearch Setup

## Repo snapshot

- [Agent bullets from scan]

## Owner priorities

### Objective

[Answer Q1]

### Run kinds (priority order)

1. kind — why
2. kind — why

### Output mode

[Answer Q3]

### Focus topics

- topic
- topic

### Avoid

- topic or constraint

### Loop cadence

[Answer Q6]

### Review handoff

[Answer Q7]

### First loop

- **Kind:** ...
- **Topic:** ...
- **Run id:** YYYY-MM-DD-<kind>-001

## Interview record

### Q1 Lab objective

**Recommendation:** ...
**Owner answer:** ...

[Repeat for Q2–Q9 and any follow-ups]
```

Then **derive config from setup** (still only under `<lab>/`):

- `<lab>/config/objective.md` ← Q1 + success criteria
- `<lab>/config/queries/seeds.json` ← focus topics as initial seeds
- `<lab>/config/sources/seeds.json` ← 5–10 sources relevant to focus topics (web search OK)
- `<lab>/config/styles/voice.md` ← only if output mode includes public drafts
- `<lab>/notes/autoresearch.md` ← objective + enabled kinds + link to setup
- `<lab>/.gitignore` ← generated from Q9 answers (section 5 defaults + owner overrides)
- **If Q10b = `inline` or `both`:** `<lab>/dashboard/index.html` ← agent-designed viewer; update after runs
- **If Q10b = `guided` or `both`:** `<lab>/dashboard/SPEC.md` + `<lab>/dashboard/BUILD-GUIDE.md` ← agent customizes the template at `start/viewer/BUILD-GUIDE.template.md`
- **If Q10b = `skip`:** no viewer files — runs live in `runs/` only

Append to `<lab>/runs/autoresearch.jsonl`:

```json
{"timestamp":"2026-05-31T12:00:00Z","event":"setup_completed","kinds":["product-bets","strategy"],"firstLoopTopic":"..."}
```

### 3F. First-start agent prompt

Use when the owner says "start autoresearch" in a fresh repo:

```markdown
Read and follow: start/AUTORESEARCH-START.md (standalone lab) or autoresearch/AUTORESEARCH-START.md (embedded)

Phase 1 — Setup (required):
1. Detect mode and resolve `<lab>/` paths (section 2)
2. Scan read-only (section 3B)
3. Bootstrap `<lab>/` scaffold if missing (section 4) — config files can be stubs
4. Interview the owner (section 3C) with pre-filled recommendations
5. Wait for answers; write `<lab>/config/setup.md` and derive config (section 3E)
6. Do NOT run loop 001 until setup status is complete

Phase 2 — First loop (after setup):
7. Run loop 001 using the agreed kind + topic from setup (section 9)
8. Report verdict, score, and top findings in chat
```

---

## 4. Bootstrap (First Session)

Create the lab scaffold under `<lab>/`. Adjust the folder name only if the owner
specifies one (e.g. `research-lab/` instead of `autoresearch/` in embedded mode).

### Standalone lab (this repo or a cloned sia-autoresearch lab)

When `start/AUTORESEARCH-START.md` exists, bootstrap at **repo root**:

```
./
├── start/                     ← shipped spec (read-only)
├── examples/                  ← shipped examples (read-only)
├── config/
│   ├── setup.md
│   ├── objective.md
│   ├── queries/seeds.json
│   ├── sources/seeds.json
│   └── styles/voice.md
├── notes/
│   └── autoresearch.md
├── runs/
│   └── autoresearch.jsonl
├── dashboard/                 ← optional; only if Q10b enables a viewer
├── publishing/
│   └── templates/
│       └── review_template.md
├── internal-mirror/           ← optional
├── best-of/                   ← optional
└── state/                     ← optional
```

### Embedded mode (inside another repo)

Create an `autoresearch/` subfolder:

```
autoresearch/
├── AUTORESEARCH-START.md      ← copy of this spec (optional but recommended)
├── README.md                  ← 5-line local summary (create)
├── .gitignore                 ← see section 5; generated from Q9 answers
├── config/
│   ├── setup.md               ← interview record + owner priorities (required)
│   ├── objective.md           ← what this lab optimizes for
│   ├── queries/seeds.json     ← search query seeds + weights
│   ├── sources/seeds.json     ← trusted source list
│   └── styles/voice.md        ← writing tone (if applicable)
├── notes/
│   └── autoresearch.md        ← living memory (create)
├── runs/
│   └── autoresearch.jsonl     ← run event log (create empty)
├── dashboard/                 ← optional; only if Q10b enables a viewer
├── publishing/
│   └── templates/
│       └── review_template.md
├── internal-mirror/           ← optional cached repo context
├── best-of/                   ← human-curated winners
└── state/                     ← local scratch (optional)
```

### Minimal `README.md` (create)

```markdown
# Autoresearch

Manual research lab. All artifacts live here. Do not edit files outside this folder
without explicit approval.

Start here: `start/AUTORESEARCH-START.md` (standalone) or `AUTORESEARCH-START.md` (embedded)
Living memory: `notes/autoresearch.md`
Run log: `runs/autoresearch.jsonl`
Viewer: optional — see `config/setup.md` → Q10b
```

### `config/objective.md` (create — adapt to repo)

```markdown
# Objective

Turn external signals and repo context into actionable bets for [PRODUCT / CODEBASE / GROWTH / STRATEGY].

## Success looks like

- At least one concrete bet or recommendation per strong run
- Source links preserved in every run
- Living memory updated so future runs compound

## Non-goals

- No silent edits to main repo docs or code
- No autonomous publishing or PRs from raw run output
```

### `config/queries/seeds.json` (create — adapt)

```json
{
  "objective": "Evolve search keywords for this lab's domains.",
  "seeds": [
    { "query": "REPLACE_WITH_DOMAIN_TOPIC_1", "weight": 1.0, "successes": 0, "failures": 0 },
    { "query": "REPLACE_WITH_DOMAIN_TOPIC_2", "weight": 1.0, "successes": 0, "failures": 0 }
  ]
}
```

Bump `weight` (+0.05 to +0.15) when a query leads to a kept run; bump `failures` when
a query produces discard-quality output.

### `config/sources/seeds.json` (create — adapt)

```json
[
  { "name": "Example Source", "category": "Strategy", "url": "https://example.com" }
]
```

### `config/styles/voice.md` (create if writing runs matter)

```markdown
# Voice

- Plain, specific, founder/operator tone — not corporate or generic AI prose.
- Lead with the insight; support with evidence and links.
- Internal memos: dense and strategic.
- Public drafts: compressed and readable.
```

---

## 5. Git Policy

Add `autoresearch/.gitignore`:

```gitignore
/internal-mirror/**
/runs/**
/state/**
/best-of/**
/publishing/queue/**
/publishing/approved/**
/publishing/archived/**
!/publishing/templates/
```

**Default:** commit scaffold + this guide + config + templates + living notes.
**Local-only:** individual runs, queue drafts, mirror cache, scratch state.

Repo owners may choose to commit select runs or `best-of/` — that is a human decision,
not an agent default.

---

## 6. Run Types

Pick one **kind** per run. Same artifact layout; emphasis shifts.

| Kind | Primary output | Use when |
|------|----------------|----------|
| `product-bets` | `bets.md` + `memo.md` | Product direction, feature bets, UX wedges |
| `strategy` | `memo.md` + `bets.md` | Market positioning, sequencing, moats |
| `code-research` | `memo.md` + `bets.md` | Architecture options, refactor bets, tech debt |
| `growth` | `memo.md` + `bets.md` | Channels, loops, experiments, messaging |
| `competitive-intel` | `notes.md` + `memo.md` | Competitor moves, gaps, threats |
| `founder-writing` | `memo.md` + `essay.md` | Public posts, launch notes, thought pieces |

### How to run each kind well

| Kind | Start from | What the agent should deliver |
|------|------------|-------------------------------|
| `product-bets` | one user problem, friction point, or product question | 3-5 concrete bets, evidence, tradeoffs, and what to test next |
| `strategy` | one decision, market thesis, or sequencing question | competing theses, recommendation criteria, and a crisp point of view |
| `code-research` | one architecture or implementation question | options, constraints, risks, and a recommendation frame |
| `growth` | one acquisition, activation, retention, or expansion bottleneck | ranked experiments with expected leverage and failure modes |
| `competitive-intel` | one company, category, or adjacent workflow | moves, gaps, positioning patterns, threats, and whitespace |
| `founder-writing` | one belief, observation, or narrative | memo-first synthesis, supporting evidence, and an optional public draft |

**Loop guidance:**

- `product-bets`: prefer product-specific evidence over generic "top 10 ideas" lists
- `strategy`: force the agent to compare paths, not just decorate your preferred one
- `code-research`: make constraints explicit before recommending architecture
- `growth`: rank ideas by likely leverage, not by novelty
- `competitive-intel`: map where competitors are strong, weak, and over-positioned
- `founder-writing`: write the internal memo before any essay; the public draft should follow the thesis, not invent it

**Reverse press release:** When a run implies something **shippable** (feature, product
surface, launch, major repositioning), add a `## Reverse press release` section to
`memo.md` and/or `essay.md` per section 8. Skip for pure intel, unsettled thesis, or
internal-only refactors with no user narrative.

---

## 7. Run Folder Layout

Naming: `runs/YYYY-MM-DD-<kind>-NNN/`

Example: `runs/2026-05-31-product-bets-001/`

Every substantial run includes:

```
runs/YYYY-MM-DD-<kind>-NNN/
├── manifest.json           ← metadata, queries, scores, verdict
├── hypotheses.md           ← candidate angles explored, ranking, rejected branches
├── notes.md                ← sources, links, raw observations
├── memo.md                 ← internal strategy / analysis memo
├── bets.md                 ← concrete bets or recommendations (if applicable)
├── essay.md                ← optional public draft (only if signal is strong)
├── score.json              ← numeric scores
├── verdict.md              ← keep / discard / send_for_review + rationale
└── voice-observations.md   ← what worked or failed in tone (writing runs)
```

Skip `essay.md` when the run is internal-only or signal is weak.

---

## 8. Artifact Templates

### `manifest.json`

```json
{
  "id": "2026-05-31-product-bets-001",
  "kind": "product-bets",
  "title": "Short title of the run thesis",
  "topic": "One-line topic statement",
  "verdict": "draft",
  "hypothesisCounts": {
    "generated": 0,
    "researched": 0,
    "discarded": 0,
    "kept": 1
  },
  "selectedQueries": ["query one", "query two"],
  "selectedSources": ["Source A", "Source B"],
  "scores": {
    "novelty": 0.0,
    "actionability": 0.0,
    "strategicLeverage": 0.0,
    "evidenceQuality": 0.0,
    "clarity": 0.0,
    "overall": 0.0
  },
  "publishingFile": null,
  "outputs": {
    "reversePressRelease": false
  },
  "createdAt": "2026-05-31T12:00:00Z",
  "updatedAt": "2026-05-31T12:00:00Z"
}
```

**Verdict values:** `discard` | `draft` | `keep` | `send_for_review`

For code/growth runs, treat `clarity` as "how legible the recommendation is to the
team." Omit or repurpose `clarity` in `score.json` if irrelevant — stay consistent
within a repo once chosen.

### `hypotheses.md`

Create this for every substantial run. This is the run's proof-of-work artifact.
It records the candidate directions the agent explored before finalizing the memo.
Do **not** dump private chain-of-thought. Record concise decision-quality summaries:
candidate angle, supporting signal, reason discarded or advanced.

```markdown
# Hypotheses

## Search strategy

- Starting question:
- Default search tools used:
- Query optimization notes:

## Candidate hypotheses generated

1. Hypothesis title — one-line claim
2. Hypothesis title — one-line claim
3. Hypothesis title — one-line claim

## Shortlisted for evidence collection

### Candidate A
- Why it looked promising:
- Queries run:
  - query one
  - query two
- Early evidence:
  - observation
- Status: advanced | discarded
- Why:

### Candidate B
- Why it looked promising:
- Queries run:
  - query three
- Early evidence:
  - observation
- Status: advanced | discarded
- Why:

## Winning thesis

- Selected hypothesis:
- Why it won:
- What lost and why:
  - hypothesis x — reason
  - hypothesis y — reason
```

### `score.json`

Use 0.00–1.00 for each dimension. **Overall** is your honest rollup, not a math
formula agents must share.

```json
{
  "novelty": 0.65,
  "actionability": 0.80,
  "strategicLeverage": 0.75,
  "evidenceQuality": 0.70,
  "clarity": 0.72,
  "overall": 0.74
}
```

Rough guide:

- **0.00–0.29** — noise; weak, off-target, or poorly evidenced
- **0.30–0.49** — low-signal; mostly obvious, generic, or under-researched
- **0.50–0.69** — useful but incomplete; some signal, but not yet differentiated or strong enough
- **0.70–0.84** — genuinely helpful internal output; clear value, but not automatically review-worthy
- **≥ 0.85** — strong; non-obvious, well-evidenced, and clearly stronger than discarded alternatives

Scoring discipline:

- Most runs should **not** score above `0.80`.
- Good writing does **not** justify a high score by itself.
- Clarity cannot compensate for weak evidence, obvious conclusions, or low leverage.
- Reward signal, not effort. A long run can still deserve a low score.

Hard downgrade triggers:

- fewer than 3 materially distinct hypotheses explored
- fewer than 3 strong linked sources used for the final recommendation
- no meaningful discarded branch with a stated reason
- recommendation is not falsifiable, testable, or implementable
- thesis is obvious or derivative relative to the topic
- citations are weak, missing, or disconnected from the core claim

If any hard downgrade trigger applies, the run should usually score below `0.70`
unless there is a strong written reason in `verdict.md`.

### `verdict.md`

```markdown
# Verdict

**Status:** draft | keep | discard | send_for_review

**Recommendation:** One sentence.

## Why

2–4 sentences on signal quality and what to do next.

## Confidence

low | medium | medium-high | high — plus one sentence on what would change your mind.

## Proof of work

- Hypotheses generated: N
- Hypotheses researched: N
- Hypotheses discarded: N
- Queries run: N
- Sources used: N
- Why this one won: 1-3 sentences

## Why this is not higher

- Missing evidence:
- What would raise the score:
- Why this stayed draft | keep | discard | send_for_review:
```

### `notes.md`

```markdown
# Source Notes

## Queries used

- query one
- query two

## Query evolution

- initial query → optimized query — why the search was refined
- initial query → optimized query — why the search was refined

## Sources

- [Title](https://...) — one-line takeaway
- [Title](https://...) — one-line takeaway

## Observations

- Bullet facts, quotes, data points
- Separate fact from inference
```

Rules:

- Always keep the exact search queries that were actually run.
- Always keep clickable links for cited web sources.
- If a query branch was unproductive, note that in `hypotheses.md` rather than deleting it.
- Prefer primary or high-signal sources over SEO summaries when possible.

### `memo.md`

```markdown
# Memo

## Thesis

One paragraph.

## Context

What is changing in the market, codebase, or user behavior.

## Analysis

Dense reasoning. Cite sources from notes.md.

## Why this thesis won

Explain why this direction beat the discarded hypotheses in `hypotheses.md`.

## Implications

What this means for the repo's product, code, growth, or strategy.

## Open questions

What still needs validation.

## Reverse press release

_Include this section when the run describes a shippable wedge, launch, or customer-visible
change. Write as if the thing already launched — Amazon Working Backwards style. Set
`manifest.json` → `outputs.reversePressRelease` to `true`._

**Headline:** [Product/feature name — customer outcome in one line]

**Subhead:** [Who it is for and the primary benefit]

**Problem:** [Customer problem in plain language — the pain before this exists]

**Solution:** [What shipped, how it works, why it matters now]

**Customer quote:** _"[Plausible customer reaction in their words]"_

**Getting started:** [How a user encounters or activates this on day one]

**FAQ (optional):**

- Q: … A: …
- Q: … A: …
```

**When to include:** `product-bets`, `strategy`, `growth`, or `founder-writing` runs where
at least one bet is concrete enough to describe as a launch. **When to skip:** early
exploration, competitive scans with no build recommendation, or code-only refactors with
no user-facing story.

### `bets.md`

```markdown
# Bets

## Bet 1: [Name]

**Claim:** One sentence.

**Build / test next:** Concrete next step.

## Bet 2: [Name]

...
```

Aim for 3–5 bets. Each bet must be falsifiable or implementable.

### `essay.md` (optional)

Write only when the memo supports a real public or stakeholder-facing draft.
Inherit arguments from `memo.md`; do not produce standalone content marketing.

For launch-adjacent or product-direction essays, you may append the same
`## Reverse press release` block at the end — written for a **public reader**, tighter
and less internal than the memo version. Do not duplicate verbatim; compress and
customer-facing-ize. Set `outputs.reversePressRelease` to `true` when either memo or
essay includes this section.

```markdown
# Essay

[Public draft body — insight-led, readable]

## Reverse press release

_Optional. Use when the essay implies something launchable; skip for pure thought pieces._

**Headline:** …
**Subhead:** …
**Problem:** …
**Solution:** …
**Customer quote:** …
**Getting started:** …
```

---

## 9. Single-Run Workflow

Execute in order:

1. **Pick kind + topic** — one sharp question, not a laundry list.
2. **Generate hypotheses** — produce multiple candidate angles before deep research.
   Target at least 5 candidates for normal runs and more when the topic is broad.
   Do not jump from the first idea straight into the memo.
3. **Optimize search branches** — self-improve the keywords, synonyms, and framing for
   each candidate using `config/queries/seeds.json` as a starting point, not a limit.
4. **Research with default search tools** — run live searches, gather evidence, and
   test the best candidate branches. Preserve the exact queries you actually used.
5. **Write `hypotheses.md`** — capture generated candidates, shortlisted branches,
   discarded branches, and why the winner advanced.
6. **Write `notes.md`** — links first, then query evolution, then observations.
7. **Write `memo.md`** — internal truth document; add `## Reverse press release` when
   the run implies a shippable wedge (section 8).
8. **Write `bets.md`** — if kind is product, strategy, code, or growth.
9. **Write `essay.md`** — only if public draft is warranted; optional reverse press
   release at end when launch-adjacent.
10. **Set output signals** — `manifest.json` → `outputs.reversePressRelease` when
   either file includes that section.
11. **Score** — fill `score.json` and mirror scores in `manifest.json`.
12. **Compare before finalizing** — check the winning thesis against the strongest
    discarded branch. If the gap is small, prefer `draft` or `keep` over inflated
    scores or premature review.
13. **Verdict** — `verdict.md` with honest status, proof-of-work counts, and why the
    score is not higher.
14. **Log** — append JSON line to `runs/autoresearch.jsonl`.
15. **Memory** — add a one-line entry to `notes/autoresearch.md`; note `[+RPR]` when
    reverse press release is present.
16. **Review file** — if `send_for_review`, create from publishing template (section 11);
    include reverse press release in draft body or link to memo section when relevant.
17. **Viewer** — if Q10b enabled one, update it (section 15); otherwise skip.
18. **Report** — post the per-run report in chat (section 14), including how many
    hypotheses were generated, tested, discarded, and why the kept thesis won.

### When stronger agents or subagents are available

Use them as accelerators, not replacements for judgment.

- Good uses:
  - parallel source gathering across distinct search branches
  - validating the strongest and second-strongest hypotheses independently
  - comparing competing summaries from different models before final scoring
- Bad uses:
  - outsourcing the final verdict without review
  - averaging multiple mediocre outputs into a fake high-confidence result
  - using more agents as a substitute for a sharper question

The parent agent is responsible for the final thesis, score, and recommendation.

### `runs/autoresearch.jsonl` entry

Append one JSON object per line:

```json
{"timestamp":"2026-05-31T12:00:00Z","event":"run_created","runId":"2026-05-31-product-bets-001","kind":"product-bets","title":"Short title","topic":"One-line topic","verdict":"draft","overallScore":0.74,"queries":["query one","query two"],"hypothesesGenerated":12,"hypothesesResearched":4,"hypothesesDiscarded":3}
```

Optional follow-up events:

```json
{"timestamp":"2026-05-31T13:00:00Z","event":"verdict_updated","runId":"2026-05-31-product-bets-001","verdict":"keep"}
```

---

## 10. Living Memory (`notes/autoresearch.md`)

Create on first run; update after every run. Keep it scannable.

```markdown
# Autoresearch Living Document

## Setup

- Status: [pending | complete] — see `config/setup.md`
- Enabled kinds: [from setup]

## Objective

[Copy from config/objective.md — one paragraph]

## Top Queries

- topic one (weight 1.05, keep 1, discard 0)
- topic two (weight 1.00, keep 0, discard 0)

## Recent Runs

- YYYY-MM-DD-kind-NNN: Title [verdict, overall score] [+RPR if reverse press release]

## Review Queue Snapshot

- Optional: list paths under publishing/queue/ awaiting human review
```

---

## 11. Publishing Review (Optional)

When a run has a stakeholder-facing draft and verdict `send_for_review`:

1. Copy `publishing/templates/review_template.md` to
   `publishing/queue/YYYY-MM-DD-<slug>.md`
2. Fill in title, audience, draft body, and why it is worth review
3. Link back to the source run folder in the review file

### `publishing/templates/review_template.md`

```markdown
# Publishing Review

**Status:** DRAFT
**Title:**
**Content Type:** essay | memo | changelog | rfc | post
**Source Run:** runs/YYYY-MM-DD-kind-NNN/

## Why This Is Worth Review

## Target Audience

## Intent

## Draft Body

## Reviewer Notes

## Revision Notes

## Feedback For Future Runs

## Publish Status

pending | approved | rejected | revise

## Verification Notes
```

Humans move approved items out of autoresearch manually. Agents do not publish.

---

## 12. Internal Mirror (Optional)

Use when runs should cite repo context without touching canonical files.

```
autoresearch/internal-mirror/
├── README.md          ← "Read-only snapshot; do not edit upstream from here"
└── ...                ← copies or symlinks of plans, architecture, decisions
```

Rules:

- **Read** mirror material during runs.
- **Cite** provenance in `notes.md` (path + date if mirrored).
- **Never** treat mirror as source of truth — upstream repo docs win.

Lightest viable mirror: copy only the 3–5 files relevant to the current objective.

---

## 13. Multi-Loop Batch (Optional)

To run N independent loops (e.g. 5 or 20):

1. Assign each subagent **one unique topic** and **one unique run id**.
2. Share only this file + `config/` + `notes/autoresearch.md` as shared context.
3. forbid duplicate topics within the batch.
4. Each subagent completes the full single-run workflow (section 9).
5. Parent agent merges: updates living memory, appends jsonl if subagents did not,
   and produces a batch summary in `notes/autoresearch.md`.
6. Parent agent re-scores conservatively. Multiple subagents can improve coverage, but
   they do not justify an inflated final score on their own.

Batch summary format:

```markdown
## YYYY-MM-DD batch

- N loops completed
- X send_for_review, Y keep, Z discard
- Top priorities:
  1. run-id — one line
  2. run-id — one line
```

---

## 14. Continuous Mode

In continuous mode the agent runs loops back-to-back and reports after each one without waiting for a new prompt.

### Enabling continuous mode

Continuous mode is active when the owner explicitly says one of:

- "run N loops" / "run 5 research loops"
- "keep going" / "continue" / "run continuously"
- `loop_cadence` in `config/setup.md` is `continuous` or a batch size (e.g. `batch-5`)

Single-on-demand (default): agent runs one loop, posts the report, then stops.

### Per-run report (required in chat after every run)

Post this block in chat immediately after completing a run:

```
## Run complete: [run-id]
**Kind:** [kind] | **Score:** [overall] | **Verdict:** [verdict]
**Topic:** [topic]
**Top findings:**
1. [finding]
2. [finding]
3. [finding]
**Run folder:** `<lab>/runs/[run-id]/`
**Viewer:** [path if enabled, or "runs/ only"]
```

### Continuous loop flow

1. Complete run N using the single-run workflow (section 9).
2. Post per-run report in chat.
3. Update viewer if Q10b enabled one (section 15).
4. Update living memory (section 10).
5. If more loops remain: pick next topic from `<lab>/config/queries/seeds.json` (highest weight, not recently used in this batch), assign next run id, continue from step 1.
6. After the final loop: post a batch summary (section 13 format).

### Pause conditions (stop even in continuous mode)

- `config/setup.md` is missing or `status: pending` → run interview first
- Owner sends any message → stop, respond, wait for new instruction
- Three consecutive discard verdicts → pause and ask if the direction should change
- Any ambiguity about the next topic → ask before continuing

---

## 15. Viewer (Optional)

**No viewer is shipped with sia-autoresearch.** Demo viewers live in `examples/`
(e.g. `reusable-rockets-thesis.html`) as reference only. During setup (Q10b), the
owner and agent choose whether and how to build a viewer for *their* lab.

| Q10b choice | What the agent does |
|-------------|---------------------|
| `skip` | Nothing — runs are read from `runs/` and chat reports |
| `inline` | Build/update `<lab>/dashboard/index.html` (section 15A) |
| `guided` | Write `<lab>/dashboard/SPEC.md` + `BUILD-GUIDE.md` (section 15B) |
| `both` | Inline viewer now + spec/guide for a richer viewer later |

The viewer is a readability layer over run files. It does not execute autoresearch
actions; the agent loop still writes files and reports in chat.

---

## 15A. Inline Viewer (when Q10b = `inline` or `both`)

If the owner chose an inline viewer, the agent maintains a self-contained HTML file at
`<lab>/dashboard/index.html`. **Design is the agent's choice** — layout, styling, and
panels should fit the repo and enabled kinds. Use `examples/` as inspiration, not as
a template to copy verbatim.

**Update after:** each run and verdict change (while inline mode remains enabled).

### Suggested content

1. **Agent picks** — runs with score ≥ 0.80 or verdict `send_for_review`
2. **All runs** — table: date, kind, title, score, verdict, links to run files
3. **Living memory snapshot** — recent entries from `notes/autoresearch.md`
4. **Activity log** — setup, run creation, verdict updates, review notes
5. **Proof of work** — hypothesis counts and why the winning thesis won (on run detail)

### Build rules

- Single `.html` file — no external dependencies (no CDN, no npm required)
- Inline CSS and JS; embed run data directly (re-embed on each update)
- Relative paths to run files so it opens locally (`open <lab>/dashboard/index.html`)
- Clean and readable — agent picks dark or light; no lorem ipsum

### Data sources

| Data | Source |
|------|--------|
| Run list | `<lab>/runs/autoresearch.jsonl` |
| Run metadata | `<lab>/runs/[run-id]/manifest.json` |
| Verdict | `<lab>/runs/[run-id]/verdict.md` |
| Memory snapshot | `<lab>/notes/autoresearch.md` |

---

## 15B. Guided Viewer Build

Use when Q10b = `guided` or `both`. The agent writes a **spec + build guide** for *this*
repo's viewer. It does **not** ship copy-pasteable app code or prescribe visual design.
A follow-up agent or owner implements the viewer in whatever stack they already use.
Reference demos: `examples/reusable-rockets-thesis.html`.

### Hard rules

- **Do not generate frontend code unprompted** unless Q10b includes `inline`. For `guided` alone, deliver `<lab>/dashboard/SPEC.md` + `BUILD-GUIDE.md`.
- **Do not pick a visual design** in guided mode — behavior, files, and workflow only.
- **Actions are described, not implemented** in the UI — the agent loop still executes writes.

### What the agent generates

1. **`<lab>/dashboard/SPEC.md`** — repo-specific. Includes:
   - Repo snapshot (1-line who/what/stage)
   - Enabled run kinds + vocabulary (use the owner's words, not generic "items")
   - Panels (3–6) with purpose and data source, including the activity log if the repo benefits from one
   - Action surface (from Q10) with the repo-local workflow each action request should map to
   - "Done looks like" — 3 bullets describing the smallest useful viewer
2. **`<lab>/dashboard/BUILD-GUIDE.md`** — derived from the template at `start/viewer/BUILD-GUIDE.template.md`. Walks the implementer through scaffolding, reading the file contracts, wiring actions, and verifying.

### File contracts the viewer reads

The viewer is just a UI over the same files agents already maintain. No new schemas.

| Surface | Read from | Notes |
|---------|-----------|-------|
| Run list | `runs/autoresearch.jsonl`, `runs/[run-id]/manifest.json` | append-only history and metadata |
| Run detail | `runs/[run-id]/hypotheses.md`, `memo.md`, `notes.md`, `bets.md`, `essay.md`, `verdict.md` | single-run evidence, proof of work, and outputs |
| Agent picks | verdict `send_for_review`/`keep` or overall ≥ 0.80 | highlight high-signal runs |
| Living memory | `notes/autoresearch.md` | rolling summary |
| Activity log | `runs/autoresearch.jsonl`, `runs/[run-id]/verdict.md`, `notes/autoresearch.md` | show setup, iterations, feedback, and follow-up requests |
| Action requests | repo-local workflow channel already used by the agent | the current agent/tooling performs the action in its next pass; the UI does not execute it |

If the repo has no established local request channel, keep the action surface informational only.

### Rebuild trigger

After every run, update `<lab>/dashboard/SPEC.md` only if enabled kinds, vocabulary,
actions, or activity-log shape changed. Regenerate `BUILD-GUIDE.md` only when the spec
changes materially. For `inline`/`both`, update `index.html` after each run.

---

## 16. Loop Starter — Agent Prompt

Copy, fill bracketed fields, and run as the agent task:

```markdown
You are running one autoresearch loop in this repo.

Read and follow: start/AUTORESEARCH-START.md (standalone lab) or autoresearch/AUTORESEARCH-START.md (embedded)

Constraints:
- Detect mode and resolve `<lab>/` paths first (section 2)
- Create and edit files ONLY under `<lab>/`
- Do not modify any existing repo files outside `<lab>/`
- Use read-only access to repo docs/code for context

Before this run:
- If `<lab>/config/setup.md` is missing or not complete → run startup interview (section 3) first; stop until owner answers
- If setup is complete → use enabled kinds and focus topics from `<lab>/config/setup.md`

This run:
- Kind: [from setup or explicit user override]
- Topic: [one sharp question]
- Run id: [YYYY-MM-DD-<kind>-NNN]

Steps:
1. Bootstrap `<lab>/` if missing (section 4)
2. Research using web + repo context; record sources in notes.md
3. Write memo.md and bets.md (or essay.md if founder-writing and signal supports it)
4. Add ## Reverse press release to memo.md and/or essay.md when the run implies something shippable; set manifest outputs.reversePressRelease
5. Score honestly; write verdict.md
6. Append autoresearch.jsonl; update notes/autoresearch.md
7. If overall ≥ 0.85 and a public draft exists, set verdict send_for_review and create publishing/queue file

Deliverables:
- List the run folder path
- State verdict and overall score
- Summarize top 1–3 bets or findings in 3 bullets
```

---

## 17. First-Run Checklist

- [ ] Mode detected; `<lab>/` paths resolved (section 2)
- [ ] Repo scan completed (section 3B)
- [ ] Owner interviewed; `<lab>/config/setup.md` has `status: complete` (includes Q10b viewer choice)
- [ ] Gitignore preferences captured (Q9); `<lab>/.gitignore` generated
- [ ] Lab scaffold exists under `<lab>/` (section 4)
- [ ] `.gitignore` excludes runs and queue by default (or per owner overrides)
- [ ] `<lab>/config/objective.md` reflects owner answers (not generic placeholders)
- [ ] Query and source seeds derived from focus topics
- [ ] `<lab>/notes/autoresearch.md` created with setup status + enabled kinds
- [ ] `<lab>/runs/autoresearch.jsonl` created (includes `setup_completed` event)
- [ ] Viewer created **only if Q10b ≠ `skip`** (inline HTML and/or spec + guide)
- [ ] First run folder has all required artifacts
- [ ] Per-run report posted in chat after run 001
- [ ] No files outside `<lab>/` were modified (standalone: `start/` and `examples/` stay read-only)

---

## 18. Promotion (Human Only)

When a run is strong enough to affect the main repo:

1. Human reviews `memo.md`, `bets.md`, and optional queue file
2. Human explicitly asks an agent to apply specific outputs to target paths
3. Agent makes **separate, deliberate edits** outside autoresearch with a normal
   commit/PR — not as a side effect of research

Autoresearch is the lab. The main repo is production.

---

## 19. Anti-Patterns

- Running loop 001 before the owner confirms setup priorities
- Filling `config/objective.md` with generic placeholders and calling setup done
- Editing canonical docs "while you're in there"
- Generating essays without memos behind them
- Reverse press releases for vague strategy with no shippable wedge
- Copy-pasting the same reverse press release block into memo and essay verbatim
- Scoring every run ≥ 0.85 to feel productive
- Building a CLI, custom app, or complex tooling before runs prove value
- Adding telemetry, analytics, or phone-home calls as part of default setup
- Duplicating the entire repo into `internal-mirror/` without cause
- Promoting bets that lack a concrete "build / test next" step

---

## 20. Quick Reference

| Question | Answer |
|----------|--------|
| First time in a repo? | Detect mode → repo scan → interview owner (sections 3B–3C) → `<lab>/config/setup.md` → then loop 001 |
| Which spec file? | `start/AUTORESEARCH-START.md` (standalone lab) or `autoresearch/AUTORESEARCH-START.md` (embedded) |
| Where does everything go? | `<lab>/` only — repo root in standalone lab; `autoresearch/` in embedded mode |
| Minimum per run? | manifest, hypotheses, notes, memo, score, verdict, jsonl entry, chat report |
| Viewer? | Optional — per Q10b in setup; demos in `examples/` |
| Where is the viewer? | `<lab>/dashboard/` if enabled; otherwise read `runs/` directly |
| When to write essay.md? | Strong signal + human-facing need (see setup output mode) |
| Reverse press release? | Optional `## Reverse press release` in memo/essay when shippable; flag in manifest |
| When to send_for_review? | Strong score + draft worth a human read |
| How does memory compound? | `<lab>/notes/autoresearch.md` + query weight updates |
| Security model? | User's agent + tools only; default web search; no repo telemetry — section 2A |
| Can I touch app code? | Only when human explicitly promotes an output |
| How do I run continuously? | User says "run N loops" or "keep going" — see section 14 |
| What goes in git? | Per Q9 answers — defaults: config, notes committed; runs and viewer local-only |

---

*Portable sia-autoresearch spec. Drop this file into any repo, run the startup interview,
then run loop 001. Source: https://github.com/sdrth/sia-autoresearch

*Inspired by [Karpathy's autoresearch](https://github.com/karpathy/autoresearch).*
