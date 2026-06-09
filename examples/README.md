# Examples

Sample sia-autoresearch runs and **demo viewers**. Use these to understand what a complete run looks like and what a viewer *could* look like — your agent chooses the actual viewer design during setup (spec section 15).

---

## `dashboard.html` — Notion-style runs viewer

**Open locally:** `open examples/dashboard.html`

A **Notion-like viewer** for browsing research runs — **no server, no CDN, no fetch**. Single self-contained HTML file with embedded data; safe to open via `file://`. Source URLs in Notes only load if you click them.

**Left sidebar**
- All runs listed with emoji, ID, score, and kind
- **Search** by title, topic, kind, or ID
- **Sort:** newest, oldest, highest/lowest score, title A–Z
- **Filter:** by kind and verdict

**Main area**
- **Highlights** at top: thesis callout, recommendation, top bet, proof of work
- **Tabbed artifacts:** Hypotheses · Notes · Memo · Bets · Verdict · Draft · Scores
- Empty tabs show a placeholder (e.g. AR-001 has no public draft; AR-002 includes a sample essay draft)

**Demo runs:** AR-001 (product-bets, example) and AR-002 (strategy)

Reference for Q10c `static-interactive` — lab viewer: `<lab>/dashboard/index.html` (static, no server).

---

## `runs/2026-05-30-product-bets-001/`

**Scenario:** A founder running sia-autoresearch for the first time in a early-stage B2B SaaS repo. They asked the agent to find product bets around onboarding drop-off.

**Kind:** `product-bets`  
**Score:** 0.76 overall  
**Verdict:** `keep`

Files included:

| File | What it contains |
|------|-----------------|
| `manifest.json` | Run metadata, queries, scores, verdict, adversarial record, claims delta |
| `hypotheses.md` | Candidate angles explored, discarded branches, and winning thesis |
| `notes.md` | Sources, links, raw observations from research |
| `memo.md` | Internal strategy memo with thesis and implications |
| `adversarial.md` | The required counter-evidence attack on the winning thesis — survivorship-bias and ICP-mismatch vectors, real counter-queries, and an honest "survived with a caveat" outcome |
| `bets.md` | 3 concrete bets with build/test next steps |
| `score.json` | Numeric scores per dimension |
| `verdict.md` | Keep/discard decision, proof of work, and why the score is not higher |

Note the adversarial outcome: the attack **did not raise the score** — surviving an
attack is the entry bar, not a bonus.

---

## `runs/2026-06-01-strategy-002/`

**Scenario:** The same founder, two days later. Run 001 found that activation is broken; now they're under market pressure to "verticalize" and ask the lab whether to pick an industry now or wait.

**Kind:** `strategy`  
**Score:** 0.72 overall  
**Verdict:** `keep`

What this run demonstrates beyond run 001:

- **Memory compounding** — the winning thesis builds on run 001's findings instead of rediscovering them
- **Counter-evidence branches** — the agent deliberately searched for "verticalization too early failure" and let it kill the strongest-sounding hypothesis
- **Adversarial pass** — `adversarial.md` formalizes the attack (sequencing-as-stall, concentration bias); the thesis survives and one bet is sharpened. The performed pass is what lets a 0.72 keep clear the spec §8 adversarial floor
- **Strategy-kind discipline** — competing theses compared, not one preferred path decorated

---

## `runs/2026-06-03-competitive-intel-003/`

**Scenario:** A landscape scan that didn't work. The founder asked "where is the whitespace in the onboarding-tool market?" — too broad a question, and vendor-side sources couldn't answer it.

**Kind:** `competitive-intel`  
**Score:** 0.38 overall  
**Verdict:** `discard`

**This is the calibration anchor for conservative scoring.** What it demonstrates:

- Real searching can still produce a discard — effort is not signal
- Weak runs are archived **cheaply**: short memo, no `bets.md`, no dressing up
- A good discard earns something: a sharper re-posed question and a retired query seed
- **The adversarial pass still runs on a discard** — `adversarial.md` steelmans that whitespace exists, fails to support it buyer-side, and so reinforces the discard rather than rescuing it; `claimsDelta` is all zero because a discard forms no durable beliefs
- The verdict explains *why nothing won* with the same rigor a keep explains why something did

If your lab's runs all score 0.75+, compare them against this pair (0.72 keep vs 0.38 discard) and recalibrate.

---

## `reusable-rockets-thesis.md`

**Scenario:** A strategy memo for SpaceX — which bold bet most lowers the cost of access to space.

**Best fit kinds:** `strategy`, `founder-writing`, `competitive-intel`

Written as a `memo.md`-style output: thesis, alternatives discarded, implications, and verdict. No meta narration about the research process — that belongs in `hypotheses.md` on a full run.

Related files:

| File | What it contains |
|------|-----------------|
| `reusable-rockets-thesis.md` | Markdown output artifact |
| `reusable-rockets-thesis.html` | Self-contained HTML viewer — **demo only**, one possible viewer style |

---

## `learning/` — self-learning loop snippets

Reference shapes for the spec 1.2.0 learning contracts (spec sections 10 and 10A):

| File | What it shows |
|------|---------------|
| `learning/bets-ledger.md` | `notes/bets-ledger.md` shape — bets with check-by dates, one resolved bet, and the running hit rate |
| `learning/meta-review.md` | A **write-back** meta-review: the file mutations it made (weights, retired seeds, claim re-verification, playbook line) plus the 5–8 observation bullets and the `meta_review` jsonl event |

The lesson in both: learning is file mutations the next run must read — a meta-review
that changes no files is incomplete.

---

## Adding your own examples

If you ran sia-autoresearch and got a strong result you want to share, open a PR:
1. Copy your run folder to `examples/runs/YYYY-MM-DD-kind-NNN/`
2. Scrub any confidential repo or company details
3. Update this README with a short scenario description
