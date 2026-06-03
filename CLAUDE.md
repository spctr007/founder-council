# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Repo Is

This is a **Claude Code skill** — a structured prompt system for running business ideas through a council of six founder-lens sub-agents. It is not a software project; there are no build steps, tests, or dependencies. The "code" is the prompt files themselves.

## File Roles

| File | Purpose |
|------|---------|
| `SKILL.md` | The skill entrypoint. Canonical orchestration logic: sizing (Lite vs Full), Evaluate vs Generate modes, the five-step pipeline, output format. This file is the source of truth for how the council runs. |
| `chairman.md` | The Chairman's system prompt. Loaded as the synthesizer's context in Step 4. Contains the blind-spot weighting table and synthesis rules. |
| `council-roster.md` | Per-seat slots injected on top of each persona: blind spot, forbidden move, must-produce, clash partners. Read this before spawning any lens. |
| `personas/*.md` | Individual founder system prompts (Jobs, Musk, Bezos, Munger, Naval, Thiel). Each file is loaded as the system prompt for that lens's sub-agent. |

## Architecture

The pipeline has six steps, numbered 0–5 (defined in `SKILL.md`):

0. **Size the council** — Lite (Naval + Munger + Chairman, single-pass) vs Full (6 lenses + peer review). Evaluate vs Generate mode.
1. **Frame the idea** — Produce a PR/FAQ from the user's raw input. Preserve three framings: Framed, Raw, Surface (Outsider only).
2. **Convene the lenses** — Spawn founder sub-agents in parallel (or sequential sections if no sub-agent support). Each receives its persona file as system prompt + the dispatch wrapper from `SKILL.md`.
3. **Peer review** — Anonymize and randomize verdicts → each lens scores others on Rigor and Decision-usefulness. De-anonymize for the Chairman.
4. **Chairman synthesis** — Load `chairman.md`, pass all verdicts de-anonymized + peer reviews → produce the structured output (Verdict, Crux, Agreement, Clashes, Blind Spots, Strongest Dissent, Recommendation, One Thing to Do First).
5. **Report + transcript** — Save `council-report-[timestamp].html` and `council-transcript-[timestamp].md` to the workspace. Lite mode skips the HTML.

## Key Design Constraints

- **Forbidden moves are load-bearing.** Each lens has a move it cannot make (`council-roster.md`). These prevent six same-model voices from collapsing into one. Never remove or soften them.
- **Peer review must be anonymized.** Reviewers score arguments, not names. De-anonymize only for the Chairman.
- **Chairman is not a seventh lens.** It has no opinion of its own — only weights the six by their documented blind spots.
- **Consensus is weak signal.** Six lenses share the same underlying model. Treat unanimous agreement as useful but not conclusive; the "most likely wrong in 6 months" peer-review answer stress-tests it.
- **Lite mode exists for low-stakes calls.** Naval + Munger + Chairman, single pass, no peer review, no HTML output.

## Output Files

After a full run, two files are saved to the workspace:
- `council-report-[timestamp].html` — self-contained HTML with inline CSS, collapsible sections per lens.
- `council-transcript-[timestamp].md` — durable record: PR/FAQ, all verdicts with flip lines, all peer reviews with A/B/C mapping revealed, Chairman synthesis.

Lite mode skips the HTML and produces a short inline verdict only.
