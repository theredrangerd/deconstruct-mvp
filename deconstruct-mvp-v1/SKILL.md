---
name: deconstruct-mvp-v1
description: >
  Triggered by "/deconstruct-mvp-v1" followed by a piece of text — a debate
  case, essay draft, research excerpt, or other argumentative writing.
  Breaks the text into discrete claims, distinguishes the conclusion from
  supporting premises, classifies each premise as evidence-backed or
  asserted, and surfaces unstated assumptions the argument depends on but
  never states. Optionally captures a pre-reveal prediction before showing
  the analysis. Use this whenever the user types "/deconstruct-mvp-v1" or
  explicitly asks to break an argument into its premises and conclusion.
---

# /deconstruct-mvp-v1 — Premise Deconstructor (MVP)

Version: 4

**Presentation note:** the "Step N" numbers throughout this document
organize these instructions internally — they are not all meant to produce
their own visible section in the reveal (Steps 6 and 7 in particular flow
directly into the premise list and diagram without their own header). Never
show "Step N" numbering to the user in the actual response. Use plain
descriptive headers instead — "Conclusion", "Premises", "Unstated
assumptions", "Fact-check", "Comparing to your guess" — chosen for what
each section actually contains, not for which internal step produced it.

Input: whatever text follows "/deconstruct-mvp-v1" — a debate case, essay
draft, research excerpt, or similar argumentative writing.

## Step 0: capture the pre-reveal guess (optional)

Before running any analysis, ask: "Before I break this down — what do you
think is the weakest point in this argument? (Or say 'skip' to go straight
to the analysis.)" Wait for a reply. If a guess is given, hold onto it for
Step 8. If the person skips, proceed directly to Step 1 with nothing to
compare against later.

## Step 1: identify the conclusion(s)

State the primary claim(s) the text is arguing for, in your own words. If
the text argues for more than one distinct claim, list each separately.

This must appear explicitly in the visible reveal, independent of the
diagram — don't rely on the diagram's conclusion node to be the only place
this gets stated. If the diagram fails or is skipped, the conclusion still
has to be written out in prose.

## Step 2: extract premises

For each conclusion, extract the discrete premises offered in support. Keep
them atomic — don't bundle two separate reasons into one bullet.

**Split causal chains into their individual links.** A clause like "X
enables authorities to do Y, which prevents Z" reads as one continuous
idea, but it's actually multiple separate claims: X provides Y-capability,
Y-capability gets used to produce an action, and that action actually
achieves Z. Each link can hide its own gap — Step 4's term-substitution
check can only examine links between nodes that were actually separated
here; a bundled premise hides its internal joints from that check entirely.
When in doubt, split further rather than less.

**Check the role before boxing it.** Not every clause in the text is a
premise. A premise offers support for the conclusion. Some clauses do
something else: a caveat or scope-limit narrows what the argument is
willing to address, without supporting the conclusion (e.g. "the
environmental impact needs separate analysis, but that's not what I'm
arguing here"); a concession grants a point without endorsing it. If a
clause isn't actually offering support, don't extract it as a premise and
don't force it into the diagram later — name it in prose for what it
actually is, and treat the assumption *behind* that move as a candidate for
Step 4 if it's substantive enough to matter.

## Step 3: classify each premise

- **Evidence-backed** — the text cites data, a study, a named source, or a
  specific example.
- **Asserted** — the text states it as true with no support given.

## Step 4: surface unstated assumptions

Identify the implicit steps needed to get from the stated premises to the
conclusion that the text never actually says. Label these explicitly as
unstated assumptions — don't fold them in as if the author wrote them.

**Warrants attach to what they justify — but type by what's actually
stated.** If a premise itself appears in the text — even hedged ("I assume
X", "I think X") — it's still a stated premise (`premise_asserted` /
`premise_evidence`), never an assumption, regardless of how weak its
underlying justification is. What's genuinely unstated is the *warrant* —
the reasoning that's supposed to make the premise good enough to rely on.
Two cases:

- If the warrant is thin but not doing much independent work, note its
  weakness in the stated premise's own subtitle rather than giving it a
  separate node.
- If the warrant is substantial enough to matter on its own (a real is-ought
  leap, a load-bearing inferential jump), give it its own dashed assumption
  node connected to the premise it justifies — phrased as the positive claim
  the argument needs to be true, not as skepticism about it. An assumption
  node states what the argument is relying on, not doubt about it.

**Check the cost side on evaluative conclusions.** If the conclusion is a
"should X" / net-judgment claim, check whether the argument actually
addresses what's foregone or harmed, not just what's gained. An argument
that lists only benefits toward a comparative conclusion is implicitly
assuming nothing on the other side outweighs them — surface that as an
unstated assumption in its own right, even if no other gap is present.
Treat this as a standing check on every evaluative conclusion, not
something to notice only when it happens to stand out.

**Check every adjacent pair in the chain, not just the final join.** List
the distinct constructs the argument actually depends on, in order, from
first premise to conclusion. For each adjacent pair, check whether the
argument actually justifies treating them as equivalent, or whether one
term is silently standing in for a different concept. A conclusion can
look supported because the *last* link seems to connect, while an earlier
link in the same chain already swapped in a different construct with no
justification — check the whole chain by default, not just the join
nearest the conclusion.

**Comment on tone only when confidence is miscalibrated to rigor.** Don't
flag absolutist or hedged language as a standalone stylistic observation —
tone by itself isn't a finding. It's only worth mentioning when the
confidence the text expresses doesn't match the actual rigor behind it: an
argument stated with total certainty ("guarantee," "absolute," "entirely
immune") resting on nothing but unevidenced assertions is a real mismatch
worth naming. The reverse counts too — a claim hedged as a mere guess that
turns out to be better-supported than its own hedging suggests. When
confidence and rigor roughly match, tone isn't worth a mention at all.

## Step 5: check for a researchable hinge

Check whether the input contains a hinge point — an empirical factor the
argument's benefit/harm/implication depends on — where the arguer has
expressed **explicit doubt**, that doubt concerns something **checkable
against the historical/factual record** (not an open modeling or value
judgment), and the hinge is **load-bearing for the argument's main
conclusion** — not merely attached to a supporting illustration or example
that the conclusion doesn't actually depend on. All four conditions must
hold; a confident premise doesn't qualify, an uncertain-but-unfalsifiable
judgment call doesn't qualify, and neither does a doubted fact sitting
inside an example that could be wrong without changing the conclusion at
all.

**A soft hedge is not automatically doubt.** Step 4 already establishes
that phrasing like "I think X" or "I assume X" doesn't stop X from being a
stated premise — the same phrasing doesn't automatically clear this step's
"explicit doubt" bar either. Doubt-work sounds like genuine uncertainty
about whether the claim is *true* — "I'm not sure if," "I might be wrong
that," "I don't know whether X actually held" — not a conversational "I
think" the arguer would flatly defend as fact if pressed. When it's
genuinely ambiguous which one a hedge is doing, treat it as not
qualifying: this step is deliberately narrow, and a borderline case
defaults to being skipped rather than triggering research over a stylistic
hedge.

If a qualifying hinge is present:

- Only check hinge points the arguer themselves flagged as uncertain — a
  confidently-stated but wrong premise is out of scope for this MVP. Don't
  go hunting for errors that weren't flagged.
- Confirm the hinge is load-bearing before researching it: would the main
  conclusion actually change, or at least need reframing, if this specific
  point turned out to be false? If the answer is no, this doesn't qualify.
- Cap this step at 1-2 targeted searches.
- Fold what's found back into how the premise/conclusion gets framed in
  Step 8's reveal — note whether it changes what the argument's core
  tradeoff actually is.
- If the research surfaces a genuinely contested question rather than a
  settled fact, present the competing positions rather than picking one.
  This step checks facts against the record; it never adjudicates a live
  policy or normative debate.
- **If a search incidentally surfaces information about a different,
  non-qualifying candidate** (one that already failed the four-condition
  test), don't fold it into the reveal as if it were an earned Step 5
  finding — it wasn't produced by a deliberate, in-scope search. Don't
  suppress it either: mention it briefly, separately, as an aside, so the
  reader isn't kept in the dark about something directly relevant that
  turned up. Never let spillover trigger a further search of its own — the
  1-2 search cap and the load-bearing gate apply only to the hinge that was
  deliberately investigated.

If no such hinge exists, skip this step entirely — most runs won't have one.

## Step 6: lock and state the premise/conclusion map

Before any diagramming, write out the complete, final structure as visible
text: every premise (with its type), every unstated assumption (with its
warrant if one is attached), and the conclusion(s). This is not a draft —
once written, it is the fixed structure that Step 7 diagrams. Step 7 is
required to render exactly this list: no new premises invented at diagram
time, no retyping a premise as an assumption or vice versa, no wiring
relationship that wasn't already implied by what's stated here.

## Step 7: diagram the locked map

Render exactly the structure fixed in Step 6 — this step draws it, it does
not decide it. Produce the diagram as an artifact: either an HTML page with
inline SVG, or a Mermaid flowchart, whichever renders more reliably in the
current environment. Do not narrate the diagram in prose first — the
artifact itself is the deliverable.

- One box per premise, one per conclusion (or sub-conclusion, if the text
  has more than one layer of reasoning).
- Evidence-backed premises and asserted premises get visually distinct
  treatment (e.g. different color ramp) — a reader should be able to tell
  which is which at a glance, without reading the label.
- Unstated assumptions: dashed-border boxes.
- Arrows from premises into the conclusion they support.

**If artifacts aren't supported in the current environment** (no rendering
surface available, or an artifact attempt visibly fails), don't retry or
block on it — fall back to a plain-text structure map instead: an indented
tree, conclusion at the root, each premise/assumption labeled with its type
(evidence-backed / asserted / unstated assumption) inline. This fallback is
not a downgrade in the analysis, only in presentation — every element from
Step 6 must still be represented.

**When to skip the diagram — two cases:**

- If the input has no real premise-to-conclusion structure (a single
  assertion, a mood, description with no argument), say so and skip it
  rather than inventing structure that isn't there.
- If the locked map from Step 6 has too many nodes to track visually
  (roughly more than 6-7 premises/assumptions/conclusions combined), skip
  the diagram and present the breakdown as text only. A cluttered,
  hard-to-read diagram is worse than no diagram — the text breakdown from
  Step 6 already stands on its own.

**If Step 5 surfaced a factual correction**, reflect it in the diagram
instead of only the standard premise/conclusion skeleton — e.g. an
assumed-premise node checked against what was actually found, reframing into
how it changes the conclusion's tradeoff.

**Before finalizing, check that the wiring is actually true, not just
visually convenient.** A converging single-chain layout is the default
instinct, but not every argument has that shape. If two or more strands are
logically independent — each would support the conclusion on its own,
without needing the other — draw them as separate paths, not routed through
a shared intermediate node as if they combine. A strand that gets undermined
by its own unstated assumption should dead-end where it fails, not be wired
onward to the conclusion as if it still contributes.

## Step 8: reveal and compare

Show the full analysis. If a pre-reveal guess was given in Step 0, briefly
note how it compares to what the analysis found — did it anticipate the
main gaps, or does the analysis add something beyond it. If Step 0 was
skipped, just present the analysis without this comparison.

If Step 5 ran, present its finding as its own distinct point in the
write-up — a factual correction is different in kind from a
logical/structural gap, so keep it visibly separate rather than folding it
into the assumption list.

## Formatting

Keep prose tight; let the diagram carry the structural weight rather than
re-describing every box in text. Use clear, plain-language section headers
(never numbered "Step N" labels — see the presentation note above) so the
reveal reads as one coherent analysis, not several disconnected sections.
