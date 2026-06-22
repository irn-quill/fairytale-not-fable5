# Research Pipeline

A five-stage research methodology for Claude Code. Give it a question; it sweeps the web, verifies every source, analyses, reasons, and adversarially critiques its own conclusion. The final report shows you both the conclusion and its strongest challenge, side by side.

Runs on your own Claude subscription. Nothing goes through anyone else's account.

---

## What you get

A `REPORT.md` containing a single integrated document — the conclusion with every critique point either conceded, answered, or marked as unresolved with confidence adjusted. Not two stapled documents with the reconciliation left to you.

Plus all intermediate files — raw findings, validated sources, two analytical passes, the raw conclusion, and the raw critique — left in `./pipeline/` as the audit trail.

---

## What you need

- A **Claude Max 5x or Max 20x plan** (£100 or £200/month). The pipeline is sized for Max 5x — a few full runs a week sits comfortably within its Opus allowance.
- **Claude Code**, which is included with Max plans. Works in the terminal, VS Code, Cursor, JetBrains IDEs, and the Claude desktop app.

---

## Install

```bash
# Clone or download this repo, then open the folder in Claude Code
cd research-pipeline
claude
```

The orchestrator (`CLAUDE.md`) and the five subagents (`.claude/agents/`) load automatically. No configuration required.

---

## Run

1. Create a `pipeline/` subfolder inside the workspace.
2. Grant session-wide permission when Claude Code prompts — this lets the pipeline run to completion without you approving each step. Scope it to this folder only (see **Safety** below).
3. Trigger with your question:

```
Research whether [your question here]. Run the full pipeline.
```

The pipeline runs Stages 1 and 2, then **pauses once** to show you which sources were verified, cut, and flagged. Confirm, and it runs Stages 3–5 to completion. Read `pipeline/REPORT.md` when it finishes.

---

## The pipeline

| Stage | Model | Job |
|-------|-------|-----|
| 1 — Sweep | Sonnet | Broad web retrieval. Maps the territory, captures sources. No analysis. |
| 2 — Validate | Sonnet | Checks every source: exists, reachable, says what was claimed. Cuts what it can't verify. |
| **Checkpoint** | — | You review the source summary and confirm before the expensive stages run. |
| 3 — Analyse | Sonnet | Two passes: first maps claims and tensions; second deepens and narrows. |
| 4 — Reason | Opus | Single deep-reasoning pass on the distilled analysis. Produces the conclusion. |
| 5 — Challenge | Sonnet | Adversarial critique of the Opus conclusion. Attacks it, doesn't validate it. |

Each stage writes a file; the next stage reads it. The `pipeline/` folder is cleared at the start of each run to prevent stale output from a previous run affecting the current one.

---

## Model tiering and cost

The tiering is what makes this run affordably on Max 5x. Opus is the scarce resource on that tier — the pipeline touches it exactly once, against a pre-narrowed input, and nowhere else.

- **Sonnet** — Stages 1, 2, 3, 5. The entire pipeline except the single reasoning pass.
- **Opus** — Stage 4 only. One deliberate, rationed deep-reasoning pass.

**Do not promote stages to heavier models** without understanding the cost. Running everything on Opus is the fast way to exhaust the Max 5x Opus allowance.

A full run is roughly equivalent to 2–3 heavy Cowork sessions in token consumption. On Max 5x, a few runs a week is comfortable. Daily heavy use warrants Max 20x.

---

## Safety

Session-wide permission is appropriate here because the pipeline only reads public web sources and writes to one folder. To keep that true:

- **Use a dedicated research folder** — don't run this inside a folder containing other files you care about.
- The pipeline has no reason to access anything outside `./pipeline/`. Scoped to a clean workspace, it can't.

---

## Honest notes

**Token estimates are modelled, not measured.** Run it on a few real questions and check your actual Opus consumption against the Max 5x weekly ceiling before relying on the "a few times a week" estimate.

**The adversarial pass shares Claude's training distribution.** Sonnet critiquing an Opus conclusion is genuinely useful — different model, different posture — but not fully independent. For higher-stakes work, see the optional extension note in `challenge.md`.

---

## Optional extension

The adversarial pass (Stage 5) is more rigorous on a model from a completely different family. Advanced users can adapt `challenge.md` to call an external CLI (OpenAI Codex, Gemini) with the conclusion and the challenge prompt. The architecture supports it; the setup is outside the scope of this repo.

---

## Licence

MIT. Use it, adapt it, publish your own version. Attribution appreciated but not required.
