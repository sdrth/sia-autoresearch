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

This is a **reference demo**. Your agent builds a viewer at `<lab>/dashboard/index.html` during setup (Q10b).

---

## `runs/2026-05-30-product-bets-001/`

**Scenario:** A founder running sia-autoresearch for the first time in a early-stage B2B SaaS repo. They asked the agent to find product bets around onboarding drop-off.

**Kind:** `product-bets`  
**Score:** 0.76 overall  
**Verdict:** `keep`

Files included:

| File | What it contains |
|------|-----------------|
| `manifest.json` | Run metadata, queries, scores, verdict |
| `hypotheses.md` | Candidate angles explored, discarded branches, and winning thesis |
| `notes.md` | Sources, links, raw observations from research |
| `memo.md` | Internal strategy memo with thesis and implications |
| `bets.md` | 3 concrete bets with build/test next steps |
| `score.json` | Numeric scores per dimension |
| `verdict.md` | Keep/discard decision, proof of work, and why the score is not higher |

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

## Adding your own examples

If you ran sia-autoresearch and got a strong result you want to share, open a PR:
1. Copy your run folder to `examples/runs/YYYY-MM-DD-kind-NNN/`
2. Scrub any confidential repo or company details
3. Update this README with a short scenario description
