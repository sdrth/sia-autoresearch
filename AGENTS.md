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
| **Viewer**            | optional — chosen at setup (Q10b); see `examples/` for demos | optional — chosen at setup                           |


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
- Viewer is optional — follow Q10b in the spec; use `examples/` as reference demos; let the owner and agent choose the design
- Do not publish or promote outputs — that is a human decision
- Search before synthesis: generate multiple hypotheses, test multiple query branches, and do not jump to the first plausible answer
- Preserve proof of work: keep the exact queries run, linked sources, discarded branches, and why the kept thesis won
- Score with restraint: most runs should not score above 0.80, and polished writing does not justify a high score by itself
- If stronger models or subagents are available, use them deliberately for bounded research or validation work, but keep final judgment local

## Continuous mode

When the user says "run N loops", "keep going", or "run continuously":

- Complete each run, post a per-run summary in chat, then continue to the next
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
- Use the agent's default search tools and optimize search queries as you learn
- Generate multiple hypotheses before writing the memo
- Record proof of work in hypotheses.md, notes.md, and verdict.md
- Score conservatively and explain why the result is not higher
- If subagents or stronger models are available, use them for targeted validation or parallel research, not to outsource the final verdict

After this run:
- Post the per-run report in chat (run id, kind, score, verdict, top 3 findings, hypotheses generated, hypotheses discarded, why the kept thesis won)
- Update the viewer only if Q10b in setup enabled one (inline, guided, or both)
```

