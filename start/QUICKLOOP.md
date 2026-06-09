# sia-autoresearch — Quick Loop Reference

**Spec version:** see `AUTORESEARCH-START.md` header.

This is the **condensed reference** for running one loop. It is not a replacement for
the full spec — read `AUTORESEARCH-START.md` once per lab (setup, security model,
templates). Use this file to stay on-spec during runs when context is tight.

If anything here conflicts with the full spec, **the full spec wins**.

---

## 0. Resolve paths first

| | Standalone lab | Embedded mode |
|---|---|---|
| Detect | `start/AUTORESEARCH-START.md` exists, no parent product repo | `autoresearch/` inside a larger project |
| `<lab>/` | repo root | `autoresearch/` |
| Spec | `start/AUTORESEARCH-START.md` | `autoresearch/AUTORESEARCH-START.md` |

Hard rules (always):

- Create/edit **only under `<lab>/`**. Standalone: `start/`, `examples/`, shipped docs are read-only.
- Surrounding repo is read-only context (embedded mode).
- No telemetry, installs, or external services. Web research = the agent's default search.
- Chat is an index — full artifacts go in `runs/`, never pasted into the conversation.
- **Speak the owner's language** — all owner-facing text mirrors the role, vocabulary,
  and KPIs in `config/styles/audience.md`. Tech terms for technical owners, marketing
  terms for marketers. Never sound foreign to your own reader (full spec §2 rule 15).

## 1. Setup gate

If `<lab>/config/setup.md` is missing or not `status: complete` → **stop and run the
startup interview** (full spec §3). Do not guess priorities. Do not run loop 001.

## 2. One loop, in order (full spec §9)

**Once per session, before run 1:** if previous `keep`/`send_for_review` runs have no
recorded owner reaction, ask **one line** ("Did you act on X?") and log an
`owner_feedback` jsonl event. Never more than one question per session (full spec §10A).

1. **Pick kind + topic** — one sharp question. Kinds: `product-bets`, `strategy`,
   `code-research`, `growth`, `competitive-intel`, `founder-writing`.
2. **Read learning memory** — `notes/claims.json` (relevant claims = priors to
   confirm or challenge), `notes/hypothesis-index.md` (don't re-research dead theses
   unless their revisit trigger fired), `notes/playbooks/<kind>.md` (tactics that
   worked).
3. **Generate ≥ 5 hypotheses** before any deep research. Do not lock onto the first
   plausible answer.
4. **Optimize queries — registry first.** Search tier-1/2 sources from
   `config/sources/sources.json` before the open web; start queries from
   `config/queries/seeds.json`, then improve.
5. **Research deeply** — multiple web-search branches; read-only repo scan when
   context exists; subagents for parallel branches when available. Keep the exact
   queries you ran.
6. **Adversarial pass (required, before scoring)** — attack the winning thesis:
   strongest counter-arguments, **≥ 2 genuine counter-evidence search branches**,
   steelman of the opposing view → `adversarial.md`. If the attack lands, revise
   `memo.md` before bets and scoring. Delegate to a subagent when available.
7. **Write artifacts** into `runs/YYYY-MM-DD-<kind>-NNN/`:

   | File | Required | Contract |
   |------|----------|----------|
   | `manifest.json` | yes | schema: `start/schemas/manifest.schema.json` (incl. `adversarial`, `claimsDelta`) |
   | `hypotheses.md` | yes | candidates, discarded branches + why, winning thesis |
   | `notes.md` | yes | exact queries, query evolution, linked sources (`src-NNN` ids), observations |
   | `memo.md` | yes | thesis → context → analysis → why it won → implications |
   | `adversarial.md` | yes | thesis attacked, counter-queries run, what survived/revised |
   | `score.json` | yes | schema: `start/schemas/score.schema.json` |
   | `verdict.md` | yes | status, proof-of-work counts, **why the score is not higher** |
   | `bets.md` | if kind warrants | 3–5 falsifiable bets + one line each in `notes/bets-ledger.md` with a check-by date |
   | `essay.md` | rare | only when the memo supports a real public draft |

8. **Score with restraint** (full spec §8):
   - Most runs land **below 0.80**. ≥ 0.85 is rare and earned.
   - Hard downgrade triggers: < 3 distinct hypotheses, < 3 strong linked sources,
     no discarded branch with a stated reason, **no real adversarial pass**,
     non-falsifiable recommendation, obvious thesis, weak citations → usually
     **below 0.70**. The adversarial trigger has no override.
   - Good writing never raises a score by itself. Reward signal, not effort.
   - Bet hit rate is a scoring input: if past high-scored runs keep resolving wrong,
     score lower (full spec §10A).
   - Calibration anchors: `examples/runs/` — a 0.76 `keep` and a low-scoring
     `discard` — plus the lab's own local anchors once they exist.
9. **Verdict** — `discard` | `draft` | `keep` | `send_for_review`. Weak runs get
   archived cheaply, not dressed up. Discarding is a valid, good outcome.
10. **Write back memory** — claim deltas to `notes/claims.json` (mirror in manifest
    `claimsDelta`), newly discarded theses to `notes/hypothesis-index.md`, strong new
    sources to `config/sources/sources.json` (`addedBy: "agent"` + provenance).
11. **Log + memory** — append `runs/autoresearch.jsonl`; one-line entry in
    `notes/autoresearch.md`. Every 5 runs, run the **write-back meta-review** (full
    spec §10): resolve due bets, apply weight updates, expire stale claims, refresh
    playbooks — a meta-review that changes no files is incomplete.
12. **Viewer** — update `<lab>/dashboard/index.html` if enabled (Q10b/Q10c). No app
    unless the owner explicitly opted in (Q10d, full spec §15C).
13. **Language check** — reread `config/styles/audience.md`; confirm memo, bets,
    verdict, and the chat report use the owner's vocabulary and KPIs, not research
    jargon. Append any new terms the owner has used since last run.
14. **Report in chat** — compact block only, written in the owner's terms:

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
**Viewer:** [path or "runs/ only"]
```

## 3. Continuous mode (full spec §14)

Active when the owner says "run N loops" / "keep going". Per loop: complete → report
compact block → update viewer + memory → next topic from seeds. **Stop** when the
owner sends any message, after 3 consecutive discards, or on any topic ambiguity.

## 4. Never do

- Run loop 001 before setup is `complete`
- Skip the adversarial pass, or fake it with zero real counter-evidence searches
- Re-research a thesis the hypothesis index discarded (unless its revisit trigger fired)
- Write bets without logging them in `notes/bets-ledger.md`
- Finish a run that changed no memory file without saying why in `verdict.md`
- Ask the owner more than one feedback question per session
- Paste `memo.md` / `hypotheses.md` / `notes.md` / full `verdict.md` into chat
- Edit files outside `<lab>/` (or `start/`, `examples/` in standalone mode)
- Score ≥ 0.85 to feel productive, or skip the "why this is not higher" section
- Report in jargon the owner never uses — mirror `config/styles/audience.md`
- Build an app or server viewer without an explicit owner yes (Q10d) — default is
  filesystem + static HTML only
- Publish, promote, or open PRs from run output — promotion is human-only (§18)

---

*Full spec: `AUTORESEARCH-START.md` in this folder. Source:
https://github.com/sdrth/sia-autoresearch*
