# Contributing to sia-autoresearch

Thanks for your interest. sia-autoresearch is a spec-first project for founders and builders. The core artifact is `start/AUTORESEARCH-START.md`, a portable agent instruction file that can run standalone or inside a repo. Contributions that make it clearer, more robust, or more useful across founder-builder workflows are welcome.

---

## Ways to contribute

### File an issue

Use GitHub Issues to:
- Report ambiguities or contradictions in the spec
- Suggest new run kinds or workflow patterns
- Share a real-world gap you hit while using autoresearch standalone or in a repo

Please describe: what you expected the agent to do, what it actually did, and which section/step was involved.

### Improve the spec

1. Fork the repo
2. Edit `start/AUTORESEARCH-START.md` (and update `CHANGELOG.md`)
3. Open a PR with a clear title and description of what changed and why
4. Keep PRs focused — one improvement per PR is easier to review

### Add examples

Real or realistic example runs are extremely valuable. See `examples/` for the expected structure. A good example includes all required run artifacts and shows an honest score + verdict.

### Fix README / AGENTS.md / Cursor rule

Small wording fixes, clearer instructions, better agent prompts — PRs welcome.

---

## Spec change principles

- **Portable first** — changes must work in any repo, not just specific stacks
- **Agent-executable** — instructions must be unambiguous enough for an agent to follow without human clarification mid-run
- **Honest over optimistic** — the spec should produce honest scores and verdicts, not feel-good outputs
- **No silent repo edits** — the hard boundary (everything under `autoresearch/` only) is non-negotiable

---

## What we won't merge

- Changes that require a specific tool, CLI, or platform to run
- Automation that publishes or promotes outputs without human review
- Anything that relaxes the "no edits outside autoresearch/" rule by default

---

## License

By contributing, you agree your contributions will be licensed under the MIT License.
