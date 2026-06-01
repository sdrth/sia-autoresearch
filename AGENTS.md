# AGENTS.md — sia-autoresearch

This file tells AI agents (GitHub Copilot, Claude, GPT, etc.) how to work with sia-autoresearch in this repo.

## What is sia-autoresearch?

sia-autoresearch is autoresearch for founders and builders. It can run as a standalone research system or inside any repo. Agents do structured research loops and produce scored, actionable outputs — product bets, strategy memos, competitive intel, growth levers, and writing drafts — without touching existing code or docs.

Full spec: `start/AUTORESEARCH-START.md`  
Source + docs: [https://github.com/sdrth/sia-autoresearch](https://github.com/sdrth/sia-autoresearch)

Inspired by [Karpathy's autoresearch](https://github.com/karpathy/autoresearch).

## Security

- Uses only the agent and tools the owner already runs — no bundled runtime
- Web research via the agent's default search; no sia-autoresearch API or telemetry
- Do not add tracking, phone-home calls, or extra installers unless the owner asks
- The owner is responsible for permissions, secrets, gitignore, and published outputs

Full model: spec section 2A.

## Path resolution (read first)

This repo is a **standalone lab**. Detect mode before reading or writing paths:


|                       | **Standalone lab (this repo)**                               | **Embedded mode (inside another repo)**              |
| --------------------- | ------------------------------------------------------------ | ---------------------------------------------------- |
| **Detect when**       | `start/AUTORESEARCH-START.md` exists                         | `autoresearch/` is a subfolder inside a product repo |
| **Spec file**         | `start/AUTORESEARCH-START.md`                                | `autoresearch/AUTORESEARCH-START.md`                 |
| **Lab root `<lab>/`** | repo root                                                    | `autoresearch/`                                      |
| **Setup**             | `config/setup.md`                                            | `autoresearch/config/setup.md`                       |
| **Runs**              | `runs/`                                                      | `autoresearch/runs/`                                 |
| **Viewer**            | static `dashboard/index.html` at setup (Q10b–c)              | same under `autoresearch/`                           |


In this repo, `start/` and `examples/` are shipped reference material — read-only unless the owner says otherwise. Demo viewers live in `examples/`.

## How to start

```
Read and follow: start/AUTORESEARCH-START.md
```

Do not skip the startup interview. The agent must confirm owner priorities and gitignore preferences before running loop 001.

Core principle: AI can run more research loops, but the human remains the distiller.

Before doing anything substantial, detect which mode you are in:

- `standalone mode` — this repo is itself the sia-autoresearch lab (`start/AUTORESEARCH-START.md` exists)
- `embedded mode` — `autoresearch/` is inside an existing product/code/content repo

In embedded mode, read the surrounding repo as read-only context.
In standalone mode, do not invent a parent repo; use `start/`, `examples/`, lab files, notes, and web research as context.

## Hard rules

- Resolve `<lab>/` from section 2 of the spec before creating files
- Create and edit only under `<lab>/` — never modify files outside it without explicit human approval
- In standalone lab mode, do not rewrite `start/`, `examples/`, or shipped public docs without approval
- **Viewer (Q10b–c):** static `dashboard/index.html` at setup unless `skip`; ask `static-simple` vs `static-interactive`; no server/npm unless owner opts in (see `examples/`)
- **Language (Q11):** write `config/styles/audience.md`; plain chat/memos/viewer for non-technical owners — no agent jargon unless they use it first; keep evidence rigorous
- Main chat is an index only — never paste full run artifacts (`memo.md`, `hypotheses.md`, `notes.md`, `bets.md`, `essay.md`, full `verdict.md`); write them under `<lab>/runs/<run-id>/` and point to paths (spec section 14)
- Go deep on every run: multiple web-search branches, read-only in-repo scanning when context exists, and subagents for parallel branches when available — parent agent keeps final synthesis and score
- Do not publish or promote outputs — that is a human decision
- Search before synthesis: generate multiple hypotheses, test multiple query branches, and do not jump to the first plausible answer
- Preserve proof of work: keep the exact queries run, linked sources, discarded branches, and why the kept thesis won
- Score with restraint: most runs should not score above 0.80, and polished writing does not justify a high score by itself
- If stronger models or subagents are available, use them proactively each run (parallel search branches, independent validation of top hypotheses), but keep final judgment local

## Run artifacts vs chat

- **Durable outputs:** full research lives in `<lab>/runs/<run-id>/` and the viewer — not in chat
- **After each run:** post only the compact block in spec section 14 (score, verdict, three one-line findings, paths); hypothesis counts in one line max
- **Do not** dump memos, hypothesis tables, source lists, or long verdict prose in the main conversation

## Continuous mode

When the user says "run N loops", "keep going", or "run continuously":

- Complete each run, post the compact per-run index in chat (never full artifacts), then continue to the next
- Stop if the user sends any message, or after 3 consecutive discard verdicts

## Quick agent prompt

```
You are running one sia-autoresearch loop in this repo.

Read and follow: start/AUTORESEARCH-START.md

Constraints:
- Detect mode and resolve `<lab>/` paths first (spec section 2)
- In this standalone lab, `<lab>/` is repo root; spec is start/AUTORESEARCH-START.md
- Create and edit files ONLY under `<lab>/`
- Do not modify start/, examples/, or other shipped public docs without approval
- Use read-only access to repo docs/code for context
- Use web search, in-repo read-only scanning, and subagents (when available) before synthesis — multiple branches every run
- Read `config/styles/audience.md` (Q11)
- Generate multiple hypotheses before writing the memo
- Record proof of work in hypotheses.md, notes.md, and verdict.md under the run folder
- Score conservatively and explain why the result is not higher
- If subagents or stronger models are available, use them proactively for parallel research and validation, not to outsource the final verdict

After this run:
- Post only the compact per-run index in chat (spec section 14) — no full memo, hypotheses, or verdict text
- Update `<lab>/dashboard/index.html` when setup chose inline or both; otherwise update guided artifacts if applicable
```

