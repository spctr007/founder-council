# Founder Council

> Run your business idea through a council of six legendary founder-lenses — Jobs, Musk, Bezos, Munger, Naval, Thiel — who analyze it independently, peer-review each other anonymously, and deliver a single decisive verdict.

Adapted from [Andrej Karpathy's LLM Council](https://x.com/karpathy) methodology. Where Karpathy ran one query past different models, this runs six **persona lenses** as sub-agents inside Claude, layering on forbidden-move constraints, anonymized peer review, and a neutral Chairman synthesis.

---

## Why a Council?

Ask one model whether your business idea is any good and you get one answer, in one voice, with one blind spot — and no way to see what it missed.

The council fixes that. Six founder-lenses each reason from a fundamentally different premise, with a documented blind spot and a move they're **forbidden** to make. They peer-review each other anonymously. Then a neutral Chairman synthesizes everything into a verdict: where they agree (and whether it's real or a shared illusion), where they clash, and what to actually do — including the one condition that would reverse the call.

**Key design insight:** when every advisor is the same underlying model wearing a different hat, their agreement is weak evidence. Three mechanisms fight this:
1. **Forbidden moves** — each lens is barred from the reasoning that would make it collapse into a neighbor
2. **Anonymized peer review** — reviewers score arguments, not names
3. **Chairman skepticism** — unanimity is a soft signal, not proof

---

## The Six Lenses

| Seat | Lens | Core Question |
|------|------|---------------|
| **Steve Jobs** | Taste & Demand | *"Should this exist at all?"* |
| **Elon Musk** | First Principles | *"What does physics permit?"* |
| **Jeff Bezos** | Customer & Durability | *"What won't change in ten years?"* |
| **Charlie Munger** | Rationality & Risk | *"How does this go to zero?"* |
| **Naval Ravikant** | Leverage & Ownership | *"How do we win without permission or armies?"* |
| **Peter Thiel** | Monopoly & Contrarian | *"What is nobody building?"* |

Plus an optional **Outsider** seat — a naive stranger who catches the curse of knowledge all six founders share.

---

## How It Works

```
Step 0  →  Size the council (Lite 3-voice or Full 6-voice; Evaluate or Generate mode)
Step 1  →  Frame the idea as a PR/FAQ with bias guards (three framings preserved)
Step 2  →  Convene the lenses (parallel sub-agents, each with its persona + constraints)
Step 3  →  Anonymized peer review (scored on Rigor and Decision-usefulness)
Step 4  →  Chairman synthesis (weighted by blind spots, crux-finding, conditional verdict)
Step 5  →  Output report (HTML) + transcript (Markdown)
```

### Two Modes

- **Evaluate** — one concrete idea on the table → judge it
- **Generate** — a problem or domain, no idea yet → invent options, then pressure-test the best ones

### Two Weights

- **Lite** — Naval + Munger + Chairman, single pass, no peer review. For reversible, low-stakes calls
- **Full** — all six lenses + peer review + Chairman. The default for real decisions

---

## Installation

This is a [Claude Code skill](https://docs.anthropic.com/en/docs/claude-code). To install:

1. Clone this repo into your Claude Code skills directory:
   ```bash
   git clone git@github.com:spctr007/founder-council.git
   ```

2. Add the skill to your Claude Code configuration, or place it in a directory Claude Code scans for skills.

3. Trigger it with any of these phrases:
   - *"council this"*
   - *"run the council"*
   - *"pressure-test this"*
   - *"stress-test this idea"*
   - *"war room this"*
   - *"debate this idea"*
   - *"should I build/launch/ship X"*
   - *"help me brainstorm a business in [domain]"* (Generate mode)

---

## Good vs Bad Council Questions

✅ **Good:** "Should I build a $97 workshop or a $497 cohort course?" · "Which of these three product ideas should I pursue?" · "Is this AI tool a business or just a feature?" · "Should I bootstrap or raise?"

❌ **Bad:** "What's Stripe's fee?" (factual) · "Write me a landing page" (creation) · "Should I name it X or Y" (no real stakes)

---

## File Structure

```
founder-council/
├── README.md                 ← you are here
├── SKILL.md                  skill entrypoint — orchestration logic and pipeline
├── chairman.md               Chairman's system prompt — synthesis rules and blind-spot weighting
├── council-roster.md         per-seat constraints: blind spot, forbidden move, must-produce, clashes
└── personas/
    ├── steve_jobs.md
    ├── elon_musk.md
    ├── jeff_bezos.md
    ├── charlie_munger.md
    ├── naval_ravikant.md
    └── peter_thiel.md        the six founder system prompts
```

---

## Output

After a full run, two files are saved to your workspace:

- **`council-report-[timestamp].html`** — self-contained HTML with inline CSS, collapsible sections per lens. Reads like a briefing.
- **`council-transcript-[timestamp].md`** — durable record: PR/FAQ, all verdicts with flip lines, peer reviews with revealed mappings, Chairman synthesis.

Lite mode produces a short inline verdict only.

---

## Credits

- **Methodology:** [Andrej Karpathy's LLM Council](https://x.com/karpathy)
- **Adaptation:** Six founder-lens sub-agents, forbidden-move roster, lite/full sizing, three-framing bias guard, Evaluate/Generate modes with divergent→convergent phase model, and PR/FAQ intake

---

## License

MIT
