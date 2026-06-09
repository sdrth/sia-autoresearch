# sia-autoresearch — Portable Start Guide

**Spec version:** 1.2.0

**Purpose:** One file an agent can follow to stand up and run sia-autoresearch as a
**standalone founder research system** or inside **any repo** for product, growth,
strategy, competitive intel, founder writing, and code research without modifying
existing repo files or docs.

**Mode:** Manual and file-based. No app, CLI, or automation required unless the repo
owner explicitly asks for it later.

**Condensed reference:** `start/QUICKLOOP.md` — the minimum viable loop in one short
file. Read this full spec once per lab (setup, security, templates); use QUICKLOOP
during runs when context is tight. This spec wins on any conflict.

**Versioning:** Record this spec version in `<lab>/config/setup.md` at bootstrap
(`specVersion` in the frontmatter). When fetching the spec from the web, prefer a
tagged release over `main` so embedded labs know which rules they follow.

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
   | **Viewer** | inline at setup by default (Q10b) | inline at setup by default (Q10b) |

   In **standalone lab** mode (this repo's layout), shipped reference material lives at
   `start/`, `examples/`, and public docs — read them, do not rewrite them without
   explicit approval. Create runtime lab artifacts at repo root: `config/`, `notes/`,
   `runs/`, `publishing/`, and `<lab>/dashboard/` (inline viewer). A viewer is **not**
   shipped with the repo; demo viewers live in `examples/`. Default at setup: agent
   builds `dashboard/index.html` (Q10b `inline`) unless the owner chooses otherwise.

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
   Ask setup interactively; use structured question tools (e.g. `AskQuestion`) when
   available. Do not guess priorities from the repo scan alone.
10. **Search before synthesis.** Generate multiple candidate hypotheses, query angles,
    and search branches before committing to a final memo. Do not lock onto the first
    plausible answer.
11. **Show proof of work.** Record how many hypotheses were considered, which search
    queries were actually used, which branches were discarded, and why the winning
    thesis beat the alternatives.
12. **Score with restraint.** A completed run is not automatically a strong run. High
    scores must be earned through non-obvious signal, evidence quality, and clear
    comparative advantage over discarded branches.
13. **Go deep on every run.** Before synthesis, run multiple web-search branches; scan
    the repo read-only when embedded or when lab notes/code exist; and use subagents or
    stronger models proactively for parallel branches when available (not optional
    extras). The parent agent remains responsible for final judgment, synthesis, and scoring.
14. **Chat is an index, not the archive.** Write full artifacts only under
    `<lab>/runs/<run-id>/` and the viewer. Post a compact per-run block in chat (section
    14) — never paste full `memo.md`, `hypotheses.md`, `notes.md`, or long `verdict.md`
    prose into the main conversation.
15. **Mirror the owner's language (Q11).** Every owner-facing output — chat reports,
    memos, bets, verdicts, viewer copy — uses the owner's own vocabulary and KPIs from
    `config/styles/audience.md`. Tech terms for technical owners, marketing terms for
    marketers, plain language for `general`. The loop must never sound foreign to its
    own reader. When the owner's messages reveal new terms, append them to
    `audience.md` vocabulary. Evidence and scoring stay rigorous regardless.
16. **No bundled network services.** This spec does not ship telemetry, analytics, or
    sia-autoresearch-specific API calls. Use only the agent, tools, and MCP servers
    the owner already runs. Web research uses the agent's **default built-in search**
    — not a custom search backend from this repo.
17. **Counter yourself before facing the owner.** Every substantial run includes a
    mandatory adversarial pass (sections 8–9): attack the winning thesis with real
    counter-evidence searches and record the attack in `adversarial.md` **before**
    scoring. An unattacked thesis caps `overall` below 0.70.
18. **Read learning memory before researching; write it back after.** Every run reads
    `notes/claims.json`, `notes/hypothesis-index.md`, and the kind's playbook before
    generating hypotheses, searches the `config/sources/sources.json` registry before
    the open web, and writes back claim deltas, discarded theses, and new sources at
    the end (sections 9–10A). A run that changes no memory must say why.

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

Ask **interactively** (one or a few questions per turn, or via structured question
tools such as `AskQuestion` when available). Do not post all questions in one message
unless the owner wants a full decision sheet.

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
- `skip` — no viewer; read runs directly from `runs/` only (owner must opt in explicitly)
- `inline` — **default:** agent builds a self-contained static `<lab>/dashboard/index.html` at setup (per Q10c) and updates it after runs (section 15A). Design is the agent's choice.
- `guided` — agent writes `<lab>/dashboard/SPEC.md` + `<lab>/dashboard/BUILD-GUIDE.md` describing what the viewer should support; a follow-up agent or owner implements it in whatever stack they use (section 15B)
- `both` — inline viewer for immediate use plus spec + guide for a richer viewer later

*Agent recommendation:* [default `inline` at setup unless the owner wants zero UI — cite `examples/` when showing what a viewer could look like; never rely on pasting full runs in chat]

**Q10c. Viewer experience** *(if Q10b ≠ `skip` — ask)*  
Simple static (`static-simple`: list + links) or static interactive (`static-interactive`: search/tabs; see `examples/dashboard.html`). One HTML file, `file://`, no server/npm. No Next/Vite/Express unless the owner explicitly opts in.

**Q10d. Local app viewer** *(mention in one line only — never push)*  
**Default: no.** The lab is filesystem + static HTML viewers only — no app, no server,
no npm. If — and only if — the owner explicitly asks for a richer local app (now or
later), follow `start/viewer/APP-GUIDE.template.md` (section 15C). Do not offer this
proactively beyond a single line, and never build it without an explicit yes.

**Q11. Reader profile — role, language, KPIs** *(every setup)*  
Build a precise picture of who reads the outputs, so the loop never sounds foreign
to its own owner. Ask pragmatically (pre-fill recommendations from the scan **and
from how the owner has written to you so far** — their messages are the best
vocabulary sample you have):

- **Role:** What do you do day to day? (founder, marketer, PM, engineer, designer,
  sales, ops, …)
- **Proficiency:** `technical` | `general` (default if unsure) | `mixed`
- **Goals:** What decisions should these research loops actually help you make?
- **KPIs:** Which 2–4 numbers do you track and care about? (e.g. MQLs, CAC,
  conversion rate for a marketer; activation, churn, MRR for a founder; latency,
  adoption, error rate for an engineer)

Write all of it to `config/styles/audience.md`, including a starting **vocabulary**
list of terms the owner actually used. Language rules that follow from it:

- **Mirror the owner's language** in chat, memos, bets, verdicts, and viewer copy —
  tech terms for technical owners, marketing terms for marketers, plain language
  for `general`. Use *their* words for their domain, not synonyms.
- **Frame findings against their KPIs** — a growth bet for a marketer talks CAC and
  conversion, not "activation funnel topology."
- No agent/research jargon unless the owner uses it first.
- Keep evidence, sources, and scores rigorous regardless of language level.

1. Post the repo snapshot, then ask Q1–Q11 interactively (see above).
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
specVersion: 1.2.0
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

### Viewer (Q10b + Q10c + Q10d)

- **Style (Q10b):** ...
- **Experience (Q10c):** `static-simple` | `static-interactive` | ...
- **Local app (Q10d):** `no` (default) | `yes — owner explicitly opted in`

### Audience (Q11)

- **Role:** ...
- **Level:** `technical` | `general` | `mixed`
- **Primary reader:** ...
- **Goals:** ...
- **KPIs:** ...
- **Vocabulary notes:** [terms observed in the owner's own messages]

[Repeat for Q2–Q9 and any follow-ups]
```

Then **derive config from setup** (still only under `<lab>/`):

- `<lab>/config/objective.md` ← Q1 + success criteria
- `<lab>/config/queries/seeds.json` ← focus topics as initial seeds
- `<lab>/config/sources/sources.json` ← living source registry seeded with 5–10 sources relevant to focus topics (web search OK); schema: `start/schemas/sources.schema.json`
- `<lab>/config/sources/README.md` ← short owner guide: how to add sources by hand (section 4)
- `<lab>/config/styles/voice.md` ← only if output mode includes public drafts
- `<lab>/config/styles/audience.md` ← Q11 role, proficiency, goals, KPIs, starting vocabulary, language rules, and learned-preferences section
- `<lab>/notes/autoresearch.md` ← objective + enabled kinds + link to setup
- `<lab>/notes/claims.json` ← empty claims ledger (section 10); schema: `start/schemas/claims.schema.json`
- `<lab>/notes/hypothesis-index.md` ← empty index of tested-and-discarded theses (section 10)
- `<lab>/notes/bets-ledger.md` ← empty bet outcome ledger (section 10)
- `<lab>/notes/playbooks/` ← empty folder; per-kind playbooks accrete at meta-reviews (section 10)
- `<lab>/.gitignore` ← generated from Q9 answers (section 5 defaults + owner overrides)
- **Q10b `inline` or `both`:** `<lab>/dashboard/index.html` (static per Q10c; seed empty at setup)
- **Q10b `guided` or `both`:** `<lab>/dashboard/SPEC.md` + `BUILD-GUIDE.md` (from `start/viewer/BUILD-GUIDE.template.md`)
- **Q10b `skip`:** no viewer files
- **Q10d `yes` (explicit owner opt-in only):** follow `start/viewer/APP-GUIDE.template.md` (section 15C); otherwise create nothing

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
8. Report only the compact per-run index in chat (section 14); full artifacts in `runs/` + viewer
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
│   ├── sources/
│   │   ├── sources.json     ← living source registry (owner-editable; agents grow it)
│   │   └── README.md        ← how the owner adds sources by hand
│   └── styles/
│       ├── audience.md      ← Q11 reader profile + learned preferences (always at setup)
│       └── voice.md         ← writing tone (if applicable)
├── notes/
│   ├── autoresearch.md      ← living memory narrative
│   ├── claims.json          ← claims ledger: what the lab currently believes
│   ├── hypothesis-index.md  ← tested-and-discarded theses (don't rediscover)
│   ├── bets-ledger.md       ← falsifiable bets + resolution outcomes
│   └── playbooks/           ← per-kind tactics that worked (meta-review only)
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
│   ├── sources/
│   │   ├── sources.json     ← living source registry (owner-editable; agents grow it)
│   │   └── README.md        ← how the owner adds sources by hand
│   └── styles/
│       ├── audience.md      ← Q11 reader profile + learned preferences (always at setup)
│       └── voice.md         ← writing tone (if applicable)
├── notes/
│   ├── autoresearch.md        ← living memory narrative (create)
│   ├── claims.json            ← claims ledger (create empty)
│   ├── hypothesis-index.md    ← discarded-thesis index (create empty)
│   ├── bets-ledger.md         ← bet outcome ledger (create empty)
│   └── playbooks/             ← per-kind playbooks (create empty folder)
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
Viewer: static `dashboard/index.html` if enabled (Q10b/Q10c); see `config/setup.md`
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

Update weights only by the explicit rule in section 10A (keep +0.05, discard −0.05,
owner `acted_on` +0.10, `ignored` on a keep −0.05; retire below 0.70, cap at 1.50).
Bump `successes`/`failures` counters as runs resolve.

### `config/sources/sources.json` (create — living source registry)

The registry replaces the old one-shot `sources/seeds.json`. It is a **living file**:
the owner can add sources by hand at any time, and agents grow it every run when they
find genuinely high-quality new sources. Schema: `start/schemas/sources.schema.json`
(self-validate by reading it — no tooling required).

```json
{
  "objective": "Trusted sources for this lab, weighted by how useful they have proven.",
  "sources": [
    {
      "id": "src-001",
      "name": "Example Industry Report",
      "url": "https://example.com/report",
      "type": "data",
      "qualityTier": 1,
      "topics": ["onboarding", "activation"],
      "weight": 1.0,
      "addedBy": "owner",
      "dateAdded": "2026-05-31",
      "lastUsed": null,
      "timesUseful": 0,
      "notes": "Primary benchmark data; prefer over secondhand summaries."
    }
  ]
}
```

Field contract:

- `id` — stable `src-NNN` id; cite it in `notes.md` so claims can chain to evidence
- `type` — `primary` | `data` | `paper` | `docs` | `news` | `community` | `blog`
- `qualityTier` — `1` (primary/authoritative) | `2` (strong secondary) | `3` (background)
- `weight` — usefulness score updated only by the section 10A rule
- `addedBy` — `owner` | `agent`; agent additions must include `notes` with provenance
  (which run found it and why it earned a slot)

Registry rules:

- **Registry-first search:** before generic web search, check the registry for
  tier-1/2 sources matching the topic and search those first (section 9).
- **Grow it every run:** when research surfaces a genuinely strong new source, append
  it with `addedBy: "agent"` — do not add SEO listicles or one-off links.
- **Owner edits win:** never delete or down-tier an `addedBy: "owner"` entry; flag
  disagreements in the meta-review instead.

### `config/sources/README.md` (create — owner guide)

```markdown
# Sources — how to add your own

`sources.json` is this lab's trusted source list. Agents check it before searching the
open web, so adding a source here steers every future research loop.

To add one, append an entry like this to the `sources` array (30 seconds, any editor):

    {
      "id": "src-NNN",          ← next free number
      "name": "Readable name",
      "url": "https://...",
      "type": "primary | data | paper | docs | news | community | blog",
      "qualityTier": 1,         ← 1 = authoritative, 2 = strong, 3 = background
      "topics": ["topic"],
      "weight": 1.0,            ← leave at 1.0; the lab adjusts it from results
      "addedBy": "owner",
      "dateAdded": "YYYY-MM-DD"
    }

Tip: tier-1 sources get searched first. The lab also adds sources it finds (marked
`addedBy: "agent"`) — feel free to re-tier or remove those; your edits always win.
```

### `config/styles/voice.md` (create if writing runs matter)

```markdown
# Voice

- Plain, specific, founder/operator tone — not corporate or generic AI prose.
- Lead with the insight; support with evidence and links.
- Internal memos: dense and strategic.
- Public drafts: compressed and readable.
```

### `config/styles/audience.md` (create at setup — from Q11)

This file is the language contract for every owner-facing output (chat reports,
memos, bets, verdicts, viewer copy). It is **living**: when the owner's messages
reveal new terms or KPIs, update the vocabulary here — do not wait for a re-setup.

```markdown
# Audience

- Role: [what the owner does — e.g. growth marketer, technical founder, PM]
- Level: general | technical | mixed
- Primary reader: [who reads outputs, if not the owner]
- Goals: [decisions the outputs should serve, in the owner's words]
- KPIs: [the 2–4 metrics they track — e.g. CAC, MQLs / activation, MRR / latency, adoption]

## Vocabulary (mirror these)

- [terms the owner actually uses — seed from setup answers and chat; append as observed]

## Avoid

- [jargon the owner never uses — agent/research jargon by default; opposing-discipline
  jargon (e.g. no infra terms for a marketer, no funnel acronyms for a backend engineer
  unless they use them)]

## Rules

- Mirror the owner's language in all owner-facing text; use their words for their domain
- Frame findings, bets, and verdicts against the KPIs above
- Plain language for general/mixed; no unexplained acronyms the owner has not used
- Keep evidence, sources, and scores rigorous regardless of language level

## Preferences (learned — update from owner_feedback events, section 10A)

- Formats that get read: [memo-first | bets-first | short verdicts | essays — observed, not assumed]
- Length tolerance: [short | medium | long]
- Kinds acted on most: [from `acted_on` feedback; steer topics toward these]
- Kinds consistently ignored: [candidates to deprioritize or reframe]
- Last updated: [date — run id or meta-review that updated this]
```

This file starts as a vocabulary contract and **becomes a preference profile**: every
`owner_feedback` event (section 10A) that reveals what the owner reads, acts on, or
ignores should update the `## Preferences` section. The lab converges on what the
owner actually uses — revealed preference beats the setup interview.

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
├── adversarial.md          ← required: counter-evidence attack on the winning thesis
├── bets.md                 ← concrete bets or recommendations (if applicable)
├── essay.md                ← optional public draft (only if signal is strong)
├── score.json              ← numeric scores
├── verdict.md              ← keep / discard / send_for_review + rationale
└── voice-observations.md   ← what worked or failed in tone (writing runs)
```

Skip `essay.md` when the run is internal-only or signal is weak. `adversarial.md` is
**required on every substantial run** — the thesis must survive a genuine attack
before it reaches the owner (sections 8 and 9).

---

## 8. Artifact Templates

### `manifest.json`

JSON Schema: `start/schemas/manifest.schema.json` — self-validate the structure
against it before finishing a run (read it; no tooling required).

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
  "adversarial": {
    "performed": true,
    "counterQueries": ["counter query one", "counter query two"],
    "thesisRevised": false,
    "summary": "One line: what attacked the thesis and what survived"
  },
  "claimsDelta": {
    "added": 0,
    "confirmed": 0,
    "revised": 0,
    "contradicted": 0
  },
  "publishingFile": null,
  "outputs": {
    "reversePressRelease": false
  },
  "specVersion": "1.2.0",
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

### `adversarial.md` (required on every substantial run)

The adversarial pass is a **contract, not a courtesy**: after the winning thesis
emerges and **before scoring**, the agent (or a dedicated subagent when available)
must attack its own thesis — strongest counter-arguments, at least **2 genuine
counter-evidence search branches**, and a steelman of the opposing view. A thesis
that has survived a real attack is categorically more trustworthy than a
first-plausible answer. Record the attack honestly; a revised or killed thesis is a
*good* outcome, not a failure.

```markdown
# Adversarial Pass

## Thesis under attack

[The winning thesis, verbatim from memo.md at the time of the attack]

## Attack vectors

1. [Strongest counter-argument — what would have to be true for the thesis to be wrong]
2. [Second vector — alternative explanation, conflicting incentive, stale data, etc.]

## Steelman of the opposing view

[The best honest case against the thesis, written as if you believed it]

## Counter-evidence queries run

- counter query one
- counter query two

## Evidence found

- [What the counter-search actually surfaced — link sources in notes.md]

## Outcome

- **Survived intact** | **Revised** | **Killed**
- What survived and why:
- What was revised (and how memo.md changed):
- Score / verdict impact: [e.g. evidenceQuality lowered 0.05; verdict moved keep → draft]
```

Rules:

- Run the counter-queries for real and keep them — they count as proof of work and are
  mirrored in `manifest.json` → `adversarial.counterQueries`.
- If the attack revises the thesis, update `memo.md` **before** writing `bets.md` and
  scoring; set `adversarial.thesisRevised` to `true`.
- A pass with zero genuine counter-evidence searched does not count as performed
  (hard downgrade trigger below).

### `score.json`

JSON Schema: `start/schemas/score.schema.json`.

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
- **Bet hit rate is a scoring input** (section 10A): if the bets-ledger shows past
  high-scored runs keep resolving `wrong`, score current runs lower until the lab's
  judgment earns the range back. A 0.78 that is right 40% of the time was not a 0.78.

Hard downgrade triggers:

- fewer than 3 materially distinct hypotheses explored
- fewer than 3 strong linked sources used for the final recommendation
- no meaningful discarded branch with a stated reason
- **no adversarial pass, or a pass with zero genuine counter-evidence searched**
  (`adversarial.md` missing, or `counterQueries` empty / not actually run)
- recommendation is not falsifiable, testable, or implementable
- thesis is obvious or derivative relative to the topic
- citations are weak, missing, or disconnected from the core claim

If any hard downgrade trigger applies, the run should usually score below `0.70`
unless there is a strong written reason in `verdict.md`. The adversarial trigger has
**no override**: a thesis that was never attacked caps `overall` below `0.70`.

Calibration anchors (shipped in `examples/runs/`):

- `2026-05-30-product-bets-001` — **0.76, keep**: actionable, multi-source, but
  moderate novelty and no product-specific data. This is what a *good normal run*
  looks like — note that it still does not reach 0.80.
- `2026-06-03-competitive-intel-003` — **0.38, discard**: real searching happened,
  but the thesis stayed obvious and under-evidenced. The verdict explains the
  discard cheaply instead of dressing the run up. Discarding is a valid outcome.

Local calibration anchors (after ~10 runs): at meta-review, promote this lab's own
best `keep` and clearest `discard` as additional anchors in `notes/autoresearch.md`
(`## Local calibration anchors`). Over time the lab calibrates against its own
history, not just factory examples.

Calibration self-audit (every 10 runs): blind re-score 2 random past runs **without
looking at the original scores**, then compare. If average drift exceeds `0.05`,
write a dated recalibration note in `notes/autoresearch.md` and score the next runs
against the stricter of the two readings. Score drift is silent — this is the alarm.

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

**Once per session, before the first run:** the owner feedback ritual (section 10A) —
if previous `keep` / `send_for_review` runs have no recorded reaction yet, ask **one
line** ("Did you act on [run]?") and log an `owner_feedback` event. Never more than
one question per session; skip silently if there is nothing to ask about.

Execute in order:

1. **Pick kind + topic** — one sharp question, not a laundry list.
2. **Read learning memory first** — before any new research:
   - `notes/claims.json` — list claims relevant to the topic; treat them as priors to
     confirm or challenge, not facts to restate
   - `notes/hypothesis-index.md` — do **not** re-research a discarded thesis unless
     its stated revisit trigger has fired
   - `notes/playbooks/<kind>.md` — apply tactics that already worked for this owner
3. **Generate hypotheses** — produce multiple candidate angles before deep research.
   Target at least 5 candidates for normal runs and more when the topic is broad.
   Do not jump from the first idea straight into the memo.
4. **Optimize search branches — registry first.** Check
   `config/sources/sources.json` for tier-1/2 sources matching the topic and search
   those before the open web. Then self-improve the keywords, synonyms, and framing
   for each candidate using `config/queries/seeds.json` as a starting point, not a limit.
5. **Research deeply** — run live web searches across multiple branches; scan the repo
   read-only when embedded or when local context exists; delegate distinct branches to
   subagents when available. Preserve the exact queries you actually used.
6. **Write `hypotheses.md`** — capture generated candidates, shortlisted branches,
   discarded branches, and why the winner advanced.
7. **Write `notes.md`** — links first, then query evolution, then observations. Cite
   registry ids (`src-NNN`) for sources that came from or enter the registry.
8. **Write `memo.md`** — internal truth document; add `## Reverse press release` when
   the run implies a shippable wedge (section 8).
9. **Adversarial pass (required)** — attack the winning thesis before anything
   user-facing is finalized: strongest counter-arguments, at least 2 genuine
   counter-evidence search branches, steelman of the opposing view. Write
   `adversarial.md` (section 8). If the thesis is revised or killed, update `memo.md`
   and `hypotheses.md` **now**, before bets and scoring. Delegate the attack to a
   subagent when available — a fresh adversary is more honest than self-review.
10. **Write `bets.md`** — if kind is product, strategy, code, or growth. Append each
   bet as one line to `notes/bets-ledger.md` with a concrete check-by date (section 10).
11. **Write `essay.md`** — only if public draft is warranted; optional reverse press
   release at end when launch-adjacent.
12. **Set output signals** — `manifest.json` → `outputs.reversePressRelease` when
   either file includes that section; fill `adversarial` and `claimsDelta` objects.
13. **Score** — fill `score.json` and mirror scores in `manifest.json`. Remember: a
   thesis that was never genuinely attacked caps below 0.70 (section 8).
14. **Compare before finalizing** — check the winning thesis against the strongest
    discarded branch *and* the adversarial outcome. If the gap is small, prefer
    `draft` or `keep` over inflated scores or premature review.
15. **Verdict** — `verdict.md` with honest status, proof-of-work counts, and why the
    score is not higher.
16. **Write back learning memory** — the run is not finished until memory changed:
    - `notes/claims.json` — add new claims; mark confirmed / revised / contradicted
      priors (with the contradicting evidence linked); mirror counts in
      `manifest.json` → `claimsDelta`
    - `notes/hypothesis-index.md` — one line per newly tested-and-discarded thesis,
      with the reason and a revisit trigger if discarded on temporary grounds
    - `config/sources/sources.json` — append genuinely strong new sources
      (`addedBy: "agent"` + provenance); update `lastUsed` / `timesUseful` for
      registry sources cited this run
17. **Log** — append JSON line to `runs/autoresearch.jsonl`.
18. **Memory** — add a one-line entry to `notes/autoresearch.md`; note `[+RPR]` when
    reverse press release is present.
19. **Review file** — if `send_for_review`, create from publishing template (section 11);
    include reverse press release in draft body or link to memo section when relevant.
20. **Viewer** — update `<lab>/dashboard/index.html` when enabled (Q10b/Q10c; default inline).
21. **Report** — post only the compact per-run index in chat (section 14). Do not paste
   run artifacts; hypothesis counts belong in one line with a pointer to `hypotheses.md`.

**Language check (applies to steps 8–21):** before finishing, reread
`config/styles/audience.md` and confirm the memo, bets, verdict, chat report, and
viewer copy use the owner's vocabulary and KPIs — not research jargon. If the owner
used new terms since the last run, append them to `audience.md` first.

### When stronger agents or subagents are available

Use them proactively on every run as accelerators, not replacements for judgment.

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

Learning-loop events (section 10A):

```json
{"timestamp":"2026-06-02T09:00:00Z","event":"owner_feedback","runId":"2026-05-31-product-bets-001","reaction":"acted_on","note":"shipped the activation metric bet"}
{"timestamp":"2026-06-15T10:00:00Z","event":"bet_resolved","runId":"2026-05-31-product-bets-001","bet":"Bet 1: activation metric","resolution":"right","note":"trial-to-paid moved +4pts after first-value event shipped"}
{"timestamp":"2026-06-15T10:30:00Z","event":"meta_review","runsCovered":5,"filesChanged":["config/queries/seeds.json","config/sources/sources.json","notes/bets-ledger.md","notes/playbooks/product-bets.md"],"betHitRate":0.67}
```

`reaction` values: `acted_on` | `ignored` | `disagreed` | `revisit`.
`resolution` values: `right` | `wrong` | `unresolved`.
The jsonl stays schema-less by design — these shapes are the contract in prose.

---

## 10. Living Memory (`notes/`)

Memory is a **belief system, not a log**. It has four structured layers plus the
human-readable narrative. Every run **reads** the layers relevant to its topic
(section 9 step 2) and **writes back** what it learned (step 16). Run 40 should be
visibly smarter than run 4 — if memory only accretes without changing behavior,
this section is being done wrong.

| File | What it holds | Read | Written |
|------|---------------|------|---------|
| `notes/autoresearch.md` | human-readable narrative + meta-reviews | every run | every run |
| `notes/claims.json` | what the lab currently believes (claims ledger) | step 2 | step 16 |
| `notes/hypothesis-index.md` | tested-and-discarded theses | step 2 | step 16 |
| `notes/bets-ledger.md` | falsifiable bets + outcomes | meta-review | step 10 + meta-review |
| `notes/playbooks/<kind>.md` | tactics that worked for this owner | step 2 | meta-review only |

### `notes/autoresearch.md` (narrative)

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

## Local calibration anchors

- Best keep so far: run-id (score) — why it earned it
- Clearest discard: run-id (score) — why it was archived

## Review Queue Snapshot

- Optional: list paths under publishing/queue/ awaiting human review
```

### `notes/claims.json` (claims ledger)

The structured truth underneath the narrative. One entry per **load-bearing belief**
the lab holds — not every observation, only claims that future runs should treat as
priors. Schema: `start/schemas/claims.schema.json`.

```json
{
  "claims": [
    {
      "id": "clm-001",
      "statement": "Activation speed, not pricing, is the primary trial-to-paid constraint for this product category.",
      "confidence": 0.7,
      "status": "active",
      "sourceRun": "2026-05-30-product-bets-001",
      "dateFormed": "2026-05-30",
      "lastReviewed": "2026-05-30",
      "reviewBy": "2026-08-30",
      "supersededBy": null,
      "evidence": ["src-001", "src-003"]
    }
  ]
}
```

Ledger rules:

- **Status lifecycle:** `active` → `revised` (statement updated, link `supersededBy`
  to the new claim) | `contradicted` (counter-evidence won; keep the entry — knowing
  what stopped being true is signal) | `expired` (past `reviewBy` and re-verification
  failed or was skipped at meta-review).
- **Read before research** (step 2): relevant `active` claims are priors. New runs
  must confirm, revise, or contradict them explicitly — never silently re-derive or
  silently ignore them.
- **Write deltas after verdict** (step 16): mirror counts in `manifest.json` →
  `claimsDelta`. A run that changes zero claims should say why in `verdict.md`.
- **Staleness:** every claim gets a `reviewBy` date (default: 3 months out; shorter
  for fast-moving topics). Runs flag stale claims they relied on; meta-reviews
  re-verify or expire them.
- `evidence` holds source-registry ids (`src-NNN`) — claim-level provenance chains
  back through `config/sources/sources.json` to real links in run notes.

### `notes/hypothesis-index.md` (don't rediscover)

One line per tested-and-discarded thesis, appended at step 16. Step 2 of every loop
checks it first: a dead thesis is only re-researched if its revisit trigger fired.

```markdown
# Hypothesis Index — tested and discarded

| Date | Thesis (one line) | Run | Why discarded | Revisit trigger |
|------|-------------------|-----|---------------|-----------------|
| 2026-05-30 | Pricing friction is the primary trial drop-off cause | product-bets-001 | downstream of activation failure; weak evidence | activation fixed but conversion still flat |
```

### `notes/bets-ledger.md` (judgment, measured)

Every bet written in any `bets.md` gets one line here at step 10, with a concrete
check-by date. Meta-reviews resolve due bets (section 10A) — this ledger is the only
mechanism that measures whether the lab's *judgment* (not its writing) is good.

```markdown
# Bets Ledger

| Run | Bet (one line) | Logged | Check by | Status | Resolution note |
|-----|----------------|--------|----------|--------|-----------------|
| product-bets-001 | Defining one activation metric lifts trial-to-paid | 2026-05-30 | 2026-07-01 | open | — |

## Hit rate

- Resolved: 0 right / 0 wrong / 0 unresolved — hit rate: n/a
```

### `notes/playbooks/<kind>.md` (don't relearn)

Per-kind tactics that demonstrably worked for **this owner**: query shapes that found
signal, source types that held up, memo structures behind keeps, framings the owner
acted on. Read at step 2 for every run of that kind.

Rules: updated **only at meta-review** (never mid-run), capped at **~20 lines** per
kind — it is a distillation, not a log. When adding a line would exceed the cap,
delete the weakest line.

### Meta-review (every 5 runs) — write-back, not commentary

Living memory should compound, not just append. After every 5th run, run the
meta-review as a **checklist with required file mutations** — a meta-review that
changes no files is incomplete:

1. **Resolve due bets** in `notes/bets-ledger.md` (`right` | `wrong` | `unresolved`)
   via quick re-research; update the hit rate; log one `bet_resolved` jsonl event per
   resolution. If the hit rate is poor for high-scored runs, say so — it lowers
   future scores (section 8).
2. **Apply weight updates** to `config/queries/seeds.json` and
   `config/sources/sources.json` per the explicit rule in section 10A; retire seeds
   and sources that fell below 0.70.
3. **Expire or re-verify stale claims** in `notes/claims.json` (past `reviewBy`).
4. **Refresh playbooks** in `notes/playbooks/<kind>.md` from the last 5 runs' keeps
   and owner reactions (respect the 20-line cap).
5. **Refresh local calibration anchors** in `notes/autoresearch.md` if a new best
   keep or clearest discard emerged; every 10 runs, run the blind re-score
   self-audit (section 8).
6. **Write the dated `## Meta-review` section** in `notes/autoresearch.md` (5–8
   bullets): repeating themes across keeps, dead seeds retired, scoring drift check,
   what the owner acted on or ignored, one thing the next 5 runs do differently.
7. **Log a `meta_review` jsonl event** listing the files actually changed.

Keep the latest 2–3 meta-reviews; fold older ones into a single summary line.

---

## 10A. Self-Improvement Loops

The lab learns by closing feedback loops: **every signal must change a file that the
next run is required to read.** Recording without adaptation is the failure mode this
section exists to prevent. All learning state lives in `config/` and `notes/`
(lab-local, plain files, diffable) — shipped `start/` files are never edited at runtime.

```
run output → owner feedback ┐
run output → bet outcomes   ├→ learning files (config/ + notes/) → next run reads them
run output → seed hit rates ┘
```

### Owner feedback loop

The owner's real reactions are the cheapest high-quality signal the lab can get.

- **Event:** append to `runs/autoresearch.jsonl`:
  `{"event":"owner_feedback","runId":"...","reaction":"acted_on | ignored | disagreed | revisit","note":"..."}`
- **Ritual:** at the start of each session — before the first run — if previous
  `keep` / `send_for_review` runs have no recorded reaction, ask **one line** ("Did
  you act on [run title]?"). **Never more than one question per session**; the ritual
  must stay cheaper than the signal it collects. Log whatever the owner answers, even
  "ignored".
- **Flows into:** seed/source weights (below) and the `## Preferences` section of
  `config/styles/audience.md` — formats read, length tolerance, kinds acted on.
  Revealed preference beats the setup interview.

### Weight update rule (queries and sources)

Applied at every meta-review — mechanical, no judgment calls:

| Signal | Adjustment |
|--------|------------|
| Seed/source contributed to a `keep` run | +0.05 |
| Seed/source contributed to a `discard` run | −0.05 |
| Contributed to a run the owner marked `acted_on` | +0.10 |
| Contributed to a `keep` the owner marked `ignored` | −0.05 |

Bounds: retire below **0.70** (move seeds to a `retired` list; mark sources tier-3 or
remove agent-added ones), cap at **1.50**. Step 4 of the loop searches high-weight
entries first, so weights directly steer the next run's research.

### Bet resolution (the strongest signal)

Bets are falsifiable by contract — so check them. At every meta-review, resolve due
bets in `notes/bets-ledger.md` via quick re-research, log `bet_resolved` events, and
recompute the running hit rate. The hit rate is a **scoring input** (section 8): a
lab whose 0.78-scored runs resolve `right` 40% of the time must score lower until its
judgment earns the range back. This is the only mechanism that measures whether the
lab's judgment — not its prose — is good.

### Calibration self-audit

Every 10 runs: blind re-score 2 random past runs without looking at the original
scores (section 8). Plus local anchors: the lab's own best keep and clearest discard
join the shipped `examples/runs/` anchors, so calibration tracks this lab's reality.

### What self-learning never does

- No runtime, cron, telemetry, or background process — learning happens whenever the
  owner runs loops
- No embedding stores or vector memory — flat files keep it portable and auditable
- No agent edits to shipped `start/` files — learned state is lab-local
- No score inflation from "the lab is learning" — improvement shows up as better
  hit rates and sharper runs, not higher self-grades

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

Post **only** this compact index in chat immediately after completing a run. Full memos,
hypotheses, notes, bets, essays, and verdict prose stay in the run folder and viewer.

**Do not paste** `memo.md`, `hypotheses.md`, `notes.md`, `bets.md`, `essay.md`, or
multi-paragraph `verdict.md` into the main conversation.

Write the topic and findings lines in the **owner's vocabulary and against their
KPIs** (`config/styles/audience.md`) — the report is for them, not for another agent.

```
## Run complete: [run-id]
**Kind:** [kind] | **Score:** [overall] | **Verdict:** [verdict]
**Topic:** [topic]
**Top findings:**
1. [one line]
2. [one line]
3. [one line]
**Proof:** [N] generated, [M] researched, [K] discarded — details in `hypotheses.md`
**Run folder:** `<lab>/runs/[run-id]/`
**Viewer:** `<lab>/dashboard/index.html` (or path if guided/both; or "runs/ only" if skip)
```

### Continuous loop flow

1. Complete run N using the single-run workflow (section 9).
2. Post per-run report in chat.
3. Update viewer if enabled (section 15; default inline `index.html`).
4. Update living memory (section 10).
5. If more loops remain: pick next topic from `<lab>/config/queries/seeds.json` (highest weight, not recently used in this batch), assign next run id, continue from step 1.
6. After the final loop: post a batch summary (section 13 format).

### Pause conditions (stop even in continuous mode)

- `config/setup.md` is missing or `status: pending` → run interview first
- Owner sends any message → stop, respond, wait for new instruction
- Three consecutive discard verdicts → pause and ask if the direction should change
- Any ambiguity about the next topic → ask before continuing

---

## 15. Viewer

**No viewer is shipped with sia-autoresearch.** Demo viewers live in `examples/`
(e.g. `reusable-rockets-thesis.html`, `dashboard.html`) as reference only. **Default at
setup:** agent builds `<lab>/dashboard/index.html` (Q10b `inline`) and updates it after
every run. Owner may opt into `skip`, `guided`, or `both`.

**The UI is strictly optional and owner-decided.** Three explicit tiers:

| Tier | What it is | When |
|------|-----------|------|
| **Tier 0** | No viewer — read `runs/` directly + chat reports | Q10b `skip` |
| **Tier 1** | Static `dashboard/index.html` — one file, embedded data, `file://` | **Default** (Q10b `inline`/`guided`/`both`) |
| **Tier 2** | Local app (read-mostly, filesystem-backed) | **Only on explicit owner opt-in (Q10d)** — never proposed, never default |

**Static-first:** one `index.html`, embedded data, opens via `file://` (Q10c). No dev
server, `fetch()` to `runs/`, or npm unless the owner explicitly opts in via Q10d.
The default lab is **filesystem + HTML viewers only** — no app, no server.

| Q10b choice | What the agent does |
|-------------|---------------------|
| `skip` | Nothing — runs are read from `runs/` and chat reports |
| `inline` | Build/update `<lab>/dashboard/index.html` (section 15A) |
| `guided` | Write `<lab>/dashboard/SPEC.md` + `BUILD-GUIDE.md` (section 15B) |
| `both` | Inline viewer now + spec/guide for a richer viewer later |
| Q10d `yes` (separate, explicit) | Follow `start/viewer/APP-GUIDE.template.md` (section 15C) |

The viewer is a readability layer over run files. It does not execute autoresearch
actions; the agent loop still writes files and posts only the compact index in chat.

---

## 15A. Inline Viewer (when Q10b = `inline` or `both`)

One self-contained `<lab>/dashboard/index.html` per Q10c; use `examples/` as inspiration only. Update after each run and verdict change.

### Suggested content

1. **Latest run summary** — current thesis, recommendation, score, and why it won
2. **Agent picks** — runs with score ≥ 0.80 or verdict `send_for_review`
3. **All runs** — table: date, kind, title, score, verdict, links to run files
4. **Living memory snapshot** — recent entries from `notes/autoresearch.md`
5. **Activity log** — setup, run creation, verdict updates, review notes
6. **Proof of work** — hypothesis counts and why the winning thesis won (on run detail)

### Build rules

- One `.html`, inline CSS/JS, embedded run data (re-embed each update); `file://` safe — no CDN, npm, server, or `fetch()` to `runs/`
- `static-simple`: table + links; `static-interactive`: search/sort/tabs in the same file

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
- **SPEC defaults to static HTML** unless the owner explicitly chose a server-backed stack.
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
| Run list | `runs/autoresearch.jsonl`, `runs/[run-id]/manifest.json` | append-only history and metadata (manifest carries `adversarial` + `claimsDelta` on spec ≥ 1.2.0) |
| Run detail | `runs/[run-id]/hypotheses.md`, `memo.md`, `adversarial.md`, `notes.md`, `bets.md`, `essay.md`, `verdict.md` | single-run evidence, proof of work, the adversarial pass, and outputs |
| Agent picks | verdict `send_for_review`/`keep` or overall ≥ 0.80 | highlight high-signal runs |
| Living memory | `notes/autoresearch.md` | rolling summary + local calibration anchors |
| Claims ledger *(1.2.0)* | `notes/claims.json` | what the lab believes, by status and confidence |
| Bet hit-rate *(1.2.0)* | `notes/bets-ledger.md`, `bet_resolved` events | resolved/open bets and running hit rate — the lab's track record |
| Source registry *(1.2.0)* | `config/sources/sources.json` | trusted sources by tier/topic/weight; read-only in the viewer |
| Activity log | `runs/autoresearch.jsonl`, `runs/[run-id]/verdict.md`, `notes/autoresearch.md` | show setup, iterations, feedback (`owner_feedback`), bet resolutions, meta-reviews, and follow-up requests |
| Action requests | repo-local workflow channel already used by the agent | the current agent/tooling performs the action in its next pass; the UI does not execute it |

The 1.2.0 surfaces are optional — include them when the owner cares how the lab
compounds; otherwise the run list + detail + memory remain the core. The viewer is a
UI over files agents already maintain — it reads `claims.json` / `sources.json` (and
validates them against the shipped schemas) but never writes them. No new schemas.

If the repo has no established local request channel, keep the action surface informational only.

### Rebuild trigger

After every run, update `<lab>/dashboard/SPEC.md` only if enabled kinds, vocabulary,
actions, or activity-log shape changed. Regenerate `BUILD-GUIDE.md` only when the spec
changes materially. For `inline`/`both`, update `index.html` after each run.

---

## 15C. Local App Viewer (Q10d — explicit opt-in only)

**Never built by default.** This tier exists only when the owner explicitly asks for
a richer local app — at setup (Q10d) or any time later. The agent must not propose
it beyond the single Q10d line, must not scaffold it speculatively, and must not
treat a static-viewer request as app consent.

When the owner opts in, customize `start/viewer/APP-GUIDE.template.md` into
`<lab>/dashboard/APP-GUIDE.md` and build from it. Hard boundaries (the guide expands
on these):

- **Local-first, read-mostly.** The app reads `runs/`, `notes/`, and `config/`
  straight from the filesystem. No database, no backend service, no telemetry, no
  network calls to render runs.
- **The agent stays the only writer of research.** The app's single write path is
  intent files under `<lab>/queue/` (promote / annotate / request-run) that the
  agent consumes on its next pass. The app never writes run artifacts, scores,
  claims, or registry entries.
- **The file contracts are the API.** Same files the static viewer reads (section
  15B) plus the learning files: `notes/claims.json`, `notes/bets-ledger.md`,
  `config/sources/sources.json`. No new schemas.
- **Removable without loss.** Deleting the app must lose nothing — all truth stays
  in the lab files. If the app dies, Tier 1 and Tier 0 still work.

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
2. Read learning memory first: notes/claims.json (priors), notes/hypothesis-index.md (dead theses), notes/playbooks/<kind>.md (tactics)
3. Research registry-first (config/sources/sources.json tier-1/2 before open web), then web + repo context; record sources in notes.md with src-NNN ids
4. Write memo.md, then run the adversarial pass (required): attack the thesis, 2+ counter-evidence branches, write adversarial.md; revise memo if the attack lands
5. Write bets.md (append each bet to notes/bets-ledger.md with a check-by date) or essay.md if founder-writing and signal supports it
6. Add ## Reverse press release to memo.md and/or essay.md when the run implies something shippable; set manifest outputs.reversePressRelease
7. Score honestly (no adversarial pass caps overall below 0.70); write verdict.md
8. Write back memory: claim deltas to notes/claims.json, discarded theses to hypothesis-index.md, new sources to the registry
9. Append autoresearch.jsonl; update notes/autoresearch.md
10. If overall ≥ 0.85 and a public draft exists, set verdict send_for_review and create publishing/queue file

Deliverables:
- List the run folder path
- State verdict and overall score
- Summarize top 1–3 bets or findings in 3 bullets
```

---

## 17. First-Run Checklist

- [ ] Mode detected; `<lab>/` paths resolved (section 2)
- [ ] Repo scan completed (section 3B)
- [ ] Setup complete (`config/setup.md` with Q10b–c, Q11) and `config/styles/audience.md` exists with role, proficiency, goals, KPIs, and starting vocabulary
- [ ] Gitignore preferences captured (Q9); `<lab>/.gitignore` generated
- [ ] Lab scaffold exists under `<lab>/` (section 4)
- [ ] `.gitignore` excludes runs and queue by default (or per owner overrides)
- [ ] `<lab>/config/objective.md` reflects owner answers (not generic placeholders)
- [ ] Query seeds derived from focus topics; `config/sources/sources.json` registry seeded (5–10 entries) with owner `README.md`
- [ ] Learning memory files created: `notes/claims.json`, `notes/hypothesis-index.md`, `notes/bets-ledger.md`, `notes/playbooks/`
- [ ] `<lab>/notes/autoresearch.md` created with setup status + enabled kinds
- [ ] `<lab>/runs/autoresearch.jsonl` created (includes `setup_completed` event)
- [ ] Viewer created when Q10b ≠ `skip` (inline HTML and/or spec + guide, with Q10c style captured); no app unless Q10d was an explicit yes
- [ ] First run folder has all required artifacts (including `adversarial.md`)
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
- Skipping the adversarial pass, or writing a token `adversarial.md` with no real
  counter-evidence search — an unattacked thesis is a first-plausible answer
- Re-researching a thesis the hypothesis index already discarded, with no fired
  revisit trigger
- Restating ledger claims as fresh findings instead of confirming/revising them
- Writing bets without appending them to `notes/bets-ledger.md` — unfalsified bets
  teach the lab nothing
- Meta-reviews that produce commentary but mutate no files
- Asking the owner more than one feedback question per session
- Reporting to a marketer in engineering jargon (or vice versa) — outputs that sound
  foreign to their own reader, ignoring `config/styles/audience.md`
- Treating Q11 as a one-time checkbox instead of updating vocabulary as the owner talks
- Building a CLI, custom app, or complex tooling before runs prove value — and never
  building the Q10d app without an explicit owner yes
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
| Minimum per run? | manifest, hypotheses, notes, memo, adversarial, score, verdict, jsonl entry, memory write-back, chat report |
| Viewer? | Static `dashboard/index.html` by default when enabled; demos in `examples/`; local app only via explicit Q10d opt-in (section 15C) |
| Where is the viewer? | `<lab>/dashboard/` if enabled; otherwise read `runs/` directly |
| When to write essay.md? | Strong signal + human-facing need (see setup output mode) |
| Reverse press release? | Optional `## Reverse press release` in memo/essay when shippable; flag in manifest |
| When to send_for_review? | Strong score + draft worth a human read |
| Adversarial pass? | Required before scoring — attack the thesis, 2+ counter-evidence branches, `adversarial.md`; skipping caps overall below 0.70 (sections 8–9) |
| How does memory compound? | Claims ledger + hypothesis index + playbooks + bets ledger (section 10), written back every run; narrative in `notes/autoresearch.md` |
| How does the lab learn? | Section 10A — owner feedback events, bet resolution + hit rate, weight update rules, write-back meta-reviews, calibration self-audit |
| Where do trusted sources live? | `config/sources/sources.json` — owner-editable registry; agents search it first and grow it every run |
| Whose language do outputs use? | The owner's — role, vocabulary, KPIs from `config/styles/audience.md` (Q11); mirror their terms, never sound foreign to them |
| Security model? | User's agent + tools only; default web search; no repo telemetry — section 2A |
| Can I touch app code? | Only when human explicitly promotes an output |
| How do I run continuously? | User says "run N loops" or "keep going" — see section 14 |
| What goes in git? | Per Q9 answers — defaults: config, notes committed; runs and viewer local-only |

---

*Portable sia-autoresearch spec. Drop this file into any repo, run the startup interview,
then run loop 001. Source: https://github.com/sdrth/sia-autoresearch

*Inspired by [Karpathy's autoresearch](https://github.com/karpathy/autoresearch).*
