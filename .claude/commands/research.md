---
description: Run the full five-stage research pipeline on a question
---
Run the full research pipeline defined in `CLAUDE.md` on this question:
**$ARGUMENTS**
Follow the orchestrator instructions exactly:
1. Clear all `.md` and `.json` files in `./pipeline/` first (create the folder if missing).
2. Run Stage 1 (sweep, sonnet) → Stage 2 (validate, sonnet).
3. STOP at the checkpoint after Stage 2. Show me the source summary (verified / cut with reasons / flagged) and the drift assessment, and wait for my explicit go-ahead.
4. On my confirmation, run Stage 3 (analyse ×2, sonnet) → Stage 4 (reason, opus, once only) → Stage 5 (challenge, sonnet).
5. Run the Synthesis final step (sonnet): integrate conclusion and critique into a single `./pipeline/REPORT.md`, then tell me it's ready.
Do not promote any stage to a heavier model than assigned. After each stage, state one line: stage finished, file written, next stage.
