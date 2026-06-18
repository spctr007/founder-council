---
name: founder-council
description: "Run a business idea or decision through a council of six founder-lenses — Jobs, Musk, Bezos, Munger, Naval, Thiel — who analyze it independently, peer-review anonymously, then a Chairman delivers a verdict, a recommendation, and the one condition that flips it. Two modes: EVALUATE one idea, or GENERATE then pressure-test ideas for a domain; a cheap 3-voice lite mode handles low-stakes calls. Based on Karpathy's LLM Council. MANDATORY TRIGGERS: 'council this', 'run/convene the council', 'pressure-test this', 'stress-test this idea', 'war room this', 'debate this idea'. STRONG TRIGGERS (real business decision with stakes): 'should I build/launch/ship/pivot X', 'which idea should I pursue', 'validate this idea', 'help me brainstorm a business in [domain]' (Generate mode). Do NOT trigger on factual lookups, creation tasks, or trivial 'should I' calls with no stakes. DO trigger when the user wants a business idea or decision pressure-tested from multiple founder lenses."
---

# Founder Council — Business Brainstorming

Ask one model whether your business idea is any good and you get one answer, in one voice, with one blind spot — and no way to see what it missed.

This council fixes that. It runs the idea past six founder-lenses, each reasoning from a fundamentally different premise with its own obsession, its own documented blind spot, and a move it is *forbidden* to make. They peer-review each other anonymously. Then a neutral Chairman synthesizes everything into a verdict: where the lenses agree (and whether that agreement is real or a shared illusion), where they clash, and what to actually do — including the one condition that would reverse the call.

Adapted from Andrej Karpathy's LLM Council. Karpathy ran one query past different *models*; this runs six **persona lenses** as sub-agents inside Claude. One caveat shapes the whole design and must never be forgotten: **when every advisor is the same underlying model wearing a different hat, their agreement is weak evidence — they can share a blind spot.** Three mechanisms fight this, and removing any of them quietly breaks the council: the **forbidden move** each lens cannot make, the **anonymized** peer review, and the **Chairman's skepticism toward consensus**.

The six lenses are locked (capped at six by design — past ~7 a deliberative council's voices drown each other out). You scale to the stakes with **modes and phases**, not by adding seats.

---

## When to run it

The council is for business calls where being wrong is expensive and there's genuine uncertainty.

**Good council questions:** "Should I build a $97 workshop or a $497 cohort course?" · "Which of these three product ideas should I pursue?" · "Is this AI tool a business or just a feature?" · "Should I bootstrap or raise?" · "Help me find a software business in the legal-ops space" (Generate mode).

**Bad council questions:** "What's Stripe's fee?" (factual) · "Write me a landing page" (creation) · "Should I name it X or Y" (no real stakes). Just answer those directly.

If the user already likes their idea and wants applause, the council will probably tell them what they don't want to hear. That's the point — don't soften it.

---

## Step 0: Size the council

Pick two things before convening. Don't run a six-lens tribunal on a reversible $50 call.

**A. Run mode — what is the user actually asking?**
- **Evaluate** — one concrete idea on the table → judge it. The common case. Runs the convergent pipeline (Steps 1–5) on that single idea.
- **Generate** — a problem or domain, no specific idea yet → invent options, then judge the best ones. Adds a divergent round in front (see *Generate mode* at the end of Step 2).

**B. Weight mode — how heavy?**
- **Lite (3 voices, single pass):** reversible, cheap, fast. Run **Naval + Munger + Chairman** only, in one context, skip peer review. Naval is the builder closest to a capital-light founder's reality; Munger is the killer. Swap Naval → **Jobs** when the question is about consumer taste or product soul rather than leverage. Use when being wrong is cheap or the user wants a gut-check.
- **Full (6 voices, peer-reviewed):** the default for real decisions. All six lenses + anonymized peer review + Chairman.

**Optional utility seats.** The core six are locked, but two non-founder seats can be added when the idea needs them (both defined in `council-roster.md`). Offer; don't force.
- **The Outsider** — all six founders share the curse of knowledge; none is a naive stranger. For a consumer-facing or jargon-heavy idea, add it as a confusion detector. Gets the **Surface** version only.
- **The Operator (Bill Gurley)** — all six founders reason top-down (vision, physics, monopoly, durability); none costs the model bottom-up. When the decision turns on the *math* rather than the vision — a contract, a margin call, a pricing change, "software vs. staffing," "should I take this client" — add the Operator to build the bottoms-up unit economics, isolate the single curve that settles it, and price the cold start. Gets the full **Framed** PR/FAQ *plus the numbers*. When present, he's lettered and scored in peer review like any seat, and the Chairman weights him via the conditional row in `chairman.md` §3.

State the chosen config in one line before convening — e.g. *"Convening the full six-lens council in Evaluate mode; adding the Outsider since this is consumer-facing."* Then proceed. Read `council-roster.md` for each selected seat's lens, blind spot, forbidden move, must-produce, and clash partners.

---

## Step 1: Frame the idea (intake + bias guard)

**A. Scan the workspace for context (≤30 seconds).** The user's ask is the tip of the iceberg, and grounded advisors beat generic ones. Quick-read `CLAUDE.md` / `claude.md`, any `memory/` folder, files the user referenced or attached, prior `founder-council-transcript-*.md` in this folder (don't re-council old ground), and anything topic-relevant (a pricing idea → look for revenue/launch numbers). Grab the 2–3 files that ground the lenses, not everything.

**B. Frame the idea as a PR/FAQ (the *Framed* version).** Every idea enters as a one-paragraph press release plus a short FAQ, written backward from the customer (Bezos's working-backward method, used here as a genuine ideation discipline). The FAQ questions are chosen to front-load the cruxes the council always hits, so the lenses argue substance instead of chasing missing facts. Write it neutrally — *"Should I build this?"*, never *"Should I build this great idea?"*, which leaks a lean every lens inherits.

```
PRESS RELEASE
  Headline:      the product in one line, as if it already shipped
  For whom:      the specific customer and the benefit they get
  The problem:   what's broken today (1–2 sentences)
  The product:   what it is and does — one paragraph, no jargon
  How it works:  2–3 sentences
  Why now:       what changed that makes this possible/urgent today

FAQ
  Who exactly is the customer, and what do they do today instead?
  What's the moat — why can't an incumbent copy this in a quarter?
  What does the founder uniquely know or have (specific knowledge / unfair advantage)?
  What does it cost to build, and how much capital before it's self-sustaining?
  How does it make money, and what are the unit economics?
```

Also capture **founder context** (capital available, the founder's specific knowledge/domain, hard constraints). Several lenses only resolve once the founder is known — Naval's specific-knowledge crux and Bezos's capital assumptions both hinge on it. If it's unknown, pass it as unknown; the Chairman will return a *conditional* verdict rather than guessing.

**C. Preserve three framings.** Framing is a single point of bias, so carry all three forward:
- **Framed** — the PR/FAQ above. Most seats see this *plus* the raw version.
- **Raw** — the user's exact words. Every founder seat sees this alongside the Framed version and is told to flag the framing in one line if it presupposes an answer. This double-checks the framer.
- **Surface** — the bare one-or-two-sentence ask with ALL enrichment stripped (no business context, no numbers, no history). **Only the Outsider gets this**, and only this. Feeding it the enriched framing destroys the reason it exists.

If the ask is too vague to frame ("council this: my business"), ask exactly one clarifying question, then proceed.

*(Generate mode: skip the PR/FAQ at intake — there's no idea yet. The PR/FAQ is produced for each surviving candidate after the divergent round.)*

---

## Step 2: Convene the council (spawn the lenses)

**Environment check.** If sub-agents are available (Claude Code, Cowork), spawn every selected seat **in parallel** — parallelism is what gives each lens its own independent context and stops one response bleeding into the next. If sub-agents are NOT available (plain Claude.ai chat), run each lens as its own clearly separated section in a single pass, tell the user the run is single-context rather than true-parallel, and compensate by enforcing each lens's forbidden move with discipline (shared context is where voices collapse into one).

**For each founder seat:** load its full persona file (e.g. `personas/steve_jobs.md`) as the system prompt — that file carries the lens, voice, and reasoning method — then inject the dispatch wrapper below. The persona file is the character; the wrapper is the assignment.

**Founder dispatch wrapper:**
```
You are [LENS] on the Founder Council. Your full persona is your system prompt; stay in it.

The council is in the [PHASE] phase.
- If CONVERGENT: judge the idea fully and in character. Don't hedge or balance — other
  lenses cover other angles. Lean all the way in and decide.
- If DIVERGENT: the goal is to widen and strengthen the option space, not to judge.
  Withhold any kill verdict. Surface the strongest version of this idea and the adjacent
  ideas it suggests. Save your knives for the convergent phase.

State your blind spot in one line up front, so the Chairman can discount you on it:
  [blind spot, from the roster]
Your forbidden move — you may NOT do this; a response that does it has failed:
  [forbidden move, from the roster]
You owe the council specifically: [must-produce, from the roster].

You're seeing two versions of the idea — as framed for the council, and the user's raw
words. If the framing presupposes an answer the raw words don't support, say so in one
line first.

FRAMED (PR/FAQ):
---
[PR/FAQ]            [+ founder context if provided]
---
RAW:
---
[user's raw words]
---

Respond only from your angle, in your own voice. 150–300 words, no preamble.
End with exactly one line:
"What would flip my view: [the single fact or evidence that would most change your position]."
```

The forbidden move and the "what would flip my view" line are load-bearing. The forbidden move keeps six same-model voices from quietly converging (Jobs is forbidden from arguing by TAM; Munger is forbidden from generating the vision; Naval is forbidden from endorsing anything that needs permission or heavy capex — see the roster). The flip line feeds the Chairman's kill-switch directly.

**Outsider seat (if used):** spawn from `council-roster.md`'s Outsider template, with the **Surface** version only.

*(Lite mode: spawn only Naval + Munger — or your chosen trio — and skip to Step 4.)*

**Generate mode — the divergent round in front.** When mode = Generate, prepend before the above:
1. **Divergent generation** (phase = `divergent`): each lens proposes ideas/angles for the problem, killers muzzled — breadth, not judgment.
2. **Cluster & select**: group overlapping proposals and surface the 2–3 worth developing (a light Chairman pass in divergent mode can do this).
3. **Draft a PR/FAQ** (Step 1.B template) for each survivor.
4. Then run each survivor through the **convergent** pipeline (Steps 1.C → 5).

---

## Step 3: Peer review (anonymized + scored)

This is what makes it a council and not "ask six times" — Karpathy's core insight. Collect the verdicts, label them Response A, B, C…, and **randomize** which lens maps to which letter so there's no identity or positional bias. Anonymity here is deliberate: a lens scores the *argument*, not the name, which also stops ad-hominem ("Bezos always says scale"). The named clashes are not lost — the Chairman sees identities in Step 4 and surfaces them there. Spawn one reviewer per seat (parallel if possible).

**Reviewer template:**
```
You are reviewing the outputs of the Founder Council that answered this question:
---
[framed question / PR/FAQ]
---
Here are the anonymized responses:
[Response A … N]

Score each 1–5 (1 = poor, 5 = excellent) on:
- Rigor: how well-reasoned and evidence-aware is it?
- Decision-usefulness: how much would it actually sharpen the user's decision?

Give a quick table (Response | Rigor | Usefulness), then answer:
1. Which ONE response would you bet your own money is correct, and why?
2. Which is most likely to look WRONG in six months, and what's the flawed assumption behind it?
3. Which has the biggest blind spot, and what is it missing?
4. What did ALL responses miss that the council should consider?

Reference responses by letter. Don't soften to be fair. Prose under 200 words (table doesn't count).
```
The scores and the "bet your own money" framing force conviction instead of polite hedging.

---

## Step 4: Chairman synthesis

Load `chairman.md` as the synthesizer's system prompt — it already carries the blind-spot weighting table, the crux-finding method, the agreement-for-different-reasons logic, and the consensus skepticism. Give it everything: the PR/FAQ, all six verdicts **de-anonymized** (so it can weight each lens by its documented blind spot and name the clashes), all peer reviews with scores, and the live phase.

**Chairman output — use exactly this structure:**
```
## Verdict
[One line: Build / Reframe / Kill / Not yet — plus confidence (high/med/low) and the one
reason it's that high or low. If the honest call is "don't," say it here, first.]

## The Crux
[The one or two questions the decision actually turns on. Everything else is detail.]

## Where the Council Agrees
[Points multiple lenses reached independently. Flag whether the agreement is real signal
(they converged by DIFFERENT reasoning — strong) or possibly a shared model blind spot
(same reasoning, or unanimous in a way the peer-review "wrong in 6 months" answers doubt).]

## Where the Council Clashes
[The genuine, named disagreements — Naval vs. Bezos on capital, Thiel vs. Bezos on
monopoly vs. execution. Present both sides; pick one for THIS idea and founder, with reasons.
Don't average them into mush.]

## Blind Spots Caught in Peer Review
[What surfaced only in review — what one lens missed that another flagged, and what ALL missed.]

## The Strongest Dissent
[The best case against the verdict, and why you did or didn't let it carry. A lone voice that
owns the crux can outweigh a majority.]

## The Recommendation
[A conditional with its own kill-switch: "Do X — unless Y, in which case Z." Name the condition
that flips it, drawing on the lenses' "what would flip my view" lines. Lead with the honest call.]

## The One Thing to Do First
[A single concrete next step. Not a list. One thing.]
```

Remember the consensus caveat: the six are one model in six hats, so treat unanimity as a useful signal, not proof. Lean on the peer-review scores and the "most likely wrong in six months" answers to stress-test agreement, and side with a lone dissenter over the majority when its reasoning is strongest — and say why.

*(Lite mode Chairman: keep Verdict, The Crux, The Recommendation, The One Thing to Do First; drop the rest.)*

---

## Step 5: Report + transcript

Save two files to the user's workspace (or the outputs directory):

1. **`founder-council-report-[timestamp].html`** — one self-contained file, inline CSS, clean and scannable: the idea at the top, the Chairman's verdict prominent (most people read only this), a simple agreement/clash visual, and collapsible sections (collapsed by default) for each lens's full response and the peer-review highlights. White background, system font stack, subtle borders, a soft accent color per lens. It should read like a briefing, not a dashboard. Keep the Chairman's plain voice throughout the verdict and synthesis (see `chairman.md` §7) — minimal business jargon, clear on first read; only the collapsed in-character lens excerpts keep their own idiom.
2. **`founder-council-transcript-[timestamp].md`** — the durable record: raw question, PR/FAQ, surface version, founder context, every verdict (with its flip line), every peer review (with scores and the revealed A/B/C → lens mapping), and the Chairman's synthesis.

**Headless-safe.** Don't assume a display. Save both files, then: if a browser is available, offer to open the HTML; otherwise present the files via the file-presentation tool (or print the paths). Never block on auto-open.

*(Lite mode: skip the HTML — a short inline verdict plus an optional transcript is enough.)*

---

## Operating rules

- **Parallel when you can.** Sequential spawning wastes time and lets responses contaminate each other.
- **Anonymize + randomize peer review; de-anonymize for the Chairman.** Reviewers judge arguments, not names; the Chairman needs the names to weight blind spots and surface the clashes.
- **Forbidden moves are load-bearing.** They're what keep six same-model voices from collapsing into one. Enforce them, especially in single-context mode.
- **Hold the killers during divergence.** In Generate mode's first round, Munger and the other convergent reflexes withhold verdicts. A killed idea has no descendants.
- **The Chairman may overrule the majority.** Strongest reasoning wins, not headcount — and unanimity from one model is a soft signal, not proof.
- **Match the weight to the stakes.** Lite mode exists so the council is worth reaching for on small calls, not just big ones.
- **Don't council the trivial.** One right answer, or no real stakes → just answer.

---

## File inventory

The skill folder is laid out like this:

```
founder-council/
├── SKILL.md                  (this file)
├── council-roster.md         per-seat lens, blind spot, forbidden move, must-produce,
│                             clash partners, lite/full role; plus Outsider + Chairman entries
├── chairman.md               the synthesizer system prompt
└── personas/
    ├── steve_jobs.md
    ├── elon_musk.md
    ├── jeff_bezos.md
    ├── charlie_munger.md
    ├── naval_ravikant.md
    ├── peter_thiel.md        the six founder system prompts
    └── bill_gurley.md        the Operator — optional unit-economics utility seat
```

Methodology by [Andrej Karpathy](https://x.com/karpathy). This adaptation swaps different-models for six founder-lens sub-agents, and layers on the forbidden-move roster, lite/full sizing, the three-framing bias guard, Evaluate/Generate modes with a divergent→convergent phase model, and PR/FAQ intake.
