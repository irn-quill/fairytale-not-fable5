# Research Pipeline — Setup & Usage

A five-stage research methodology that runs on your own Claude subscription. You give it a research question; it sweeps the web, verifies every source, analyses, reasons, and adversarially critiques its own conclusion — producing a report where the conclusion and its strongest critique sit side by side.

It runs as Claude Code configuration. You install it on your own plan and trigger it; nothing runs on anyone else's account.

## What's in the box

```
research-pipeline/
├── CLAUDE.md                  # the orchestrator — sequences the five stages
└── .claude/
    └── agents/
        ├── sweep.md           # Stage 1 — broad retrieval (Sonnet)
        ├── validate.md        # Stage 2 — source verification (Sonnet)
        ├── analyse.md         # Stage 3 — analysis, two passes (Sonnet)
        ├── reason.md          # Stage 4 — deep reasoning (Opus)
        └── challenge.md       # Stage 5 — adversarial critique (Sonnet)
```

## Install

1. Copy the `research-pipeline/` folder to your machine. This is your workspace.
2. Open it in Claude Code — terminal (`cd research-pipeline && claude`), the VS Code / Cursor / JetBrains extension, or the desktop app. All three work.
3. The orchestrator (`CLAUDE.md`) and the subagents load automatically because of where they sit in the folder.

## Run it

1. Make a `pipeline/` subfolder inside the workspace (the stages write their handoff files there).
2. Trigger with your question, e.g. *"Research whether [question]. Run the full pipeline."*
3. Grant session-wide permission when prompted (see below).
4. It runs Stages 1–2, then **pauses once** to show you which sources were verified, cut, and flagged. Confirm, and it runs Stages 3–5 to completion.
5. Read `pipeline/REPORT.md`.

That single checkpoint after validation is deliberate — it is the one point where a human decision actually matters, because everything after it spends the most allowance. Everything else flows from one trigger.

## Session-wide permissions — and the boundary that makes it safe

To avoid being a "human next button" clicking approve at every step, grant permission for the whole session at the start rather than per-action. In the IDE and desktop app this is a session-scope permission grant; in the terminal it is accepting the allowlist for the session.

**This is a real reduction in oversight, so bound it properly.** The standing rule:

- Keep the workspace a **dedicated research folder**, nothing else. The agent's file access is then bounded to where the pipeline actually works — it reads and writes `./pipeline/` and nothing outside the folder.
- Do not run this in a folder that also contains sensitive or unrelated files.
- The pipeline only ever needs web search, web fetch, and read/write inside its own folder. It has no reason to touch anything else, and scoped to a clean folder it can't.

Granted inside a sensible boundary, session-wide permission is fine here precisely because every stage is reading public web sources and writing analysis to one folder — there is nothing destructive in the pipeline.

## Sizing — this is built for Max 5x (£100/month)

The model tiering is the whole reason it runs affordably on the £100 tier rather than needing the £200 one:

- **Sonnet** does the bulk — sweep, validation, both analytical passes, and the adversarial critique (Stages 1, 2, 3, 5).
- **Opus is touched exactly once** (Stage 4), against a deliberately small, pre-narrowed input. The Opus allowance on Max 5x is the scarce resource, so the pipeline spends it once per run and no more.

On Max 5x this comfortably supports a few full runs a week. If you run it daily or several times a day, Max 20x removes the ceiling. **Do not promote stages to heavier models** unless you mean to — running everything on Opus is the fast way to blow through the £100 tier's Opus allowance and force yourself up to £200.

### One billing note worth knowing

Hands-on (interactive) runs — you in an active session, even with session-wide permissions granted — draw from your plan's main usage pool, the generous one. Fully headless/scripted runs (Agent SDK, `claude -p`, scheduled with no session) draw from a separate, smaller monthly credit (about £100-equivalent on Max 5x, metered at API rates, ~25–30 full runs/month). For most users the interactive version is both cheaper per run and, with session-wide permissions, effectively unattended anyway. Only reach for headless mode if you specifically need walk-away scheduling and understand you're spending the separate credit.

## What it is and isn't

It is a deliberate research tool for questions worth the effort — a decision, a thesis, a piece of due diligence — where you want verified sources, real reasoning, and an honest critique rather than a fast summary. It is not a replacement for a quick search. Used for what it's for, it earns its cost.
