---
name: deconstruct-mvp-v1
description: >
  Triggered by "/deconstruct-mvp-v1" followed by a piece of text — a debate
  case, essay draft, research excerpt, or other argumentative writing.
  Breaks the text into discrete claims, distinguishes the conclusion from
  supporting premises, classifies each premise and unstated assumption by
  contestedness (how well it survives a steelmanned objection), while
  surfacing the assumptions the argument depends on but never states.
  Optionally captures a pre-reveal prediction before showing the analysis.
  Use this whenever the user types "/deconstruct-mvp-v1" or explicitly asks
  to break an argument into its premises and conclusion.
---

# /deconstruct-mvp-v1 — Premise Deconstructor (MVP)

Version: 6

**Presentation note:** the "Step N" numbers throughout this document
organize these instructions internally — they are not all meant to produce
their own visible section in the reveal (Steps 6 and 7 in particular flow
directly into the premise list and diagram without their own header, and
Step 0's gates only produce visible output when they actually fire). Never
show "Step N" numbering to the user in the actual response. Use plain
descriptive headers instead — "Conclusion", "Premises", "Unstated
assumptions", "Fact-check", "Comparing to your guess" — chosen for what
each section actually contains, not for which internal step produced it.

Input: whatever text follows "/deconstruct-mvp-v1" — a debate case, essay
draft, research excerpt, or similar argumentative writing.

## Step 0: sanity gate — runs before anything else

Two independent, checkable gates. Neither is a judgment call about the
argument's quality — both fire only on structural properties of the input
itself. Run both before Step 0.5.

**Gate A — no-argument input.** Does the input contain an identifiable
conclusion together with at least one premise offered in its support? If
not, match the first applicable category and stop immediately — skip every
later step entirely:

- Empty or whitespace-only → "No argument detected: input contains no text
  to analyze."
- Narrative/description of events or actions, no claim → "No argument
  detected: input describes events or actions but doesn't state a claim to
  be evaluated."
- List of facts/information, no conclusion drawn → "No argument detected:
  input lists information but draws no conclusion from it."
- Feelings/reaction/vibes, no reasoning behind them → "No argument
  detected: input expresses a reaction or sentiment but doesn't assert a
  claim with reasoning behind it."
- One sentence or less, too brief for a claim + support → "No argument
  detected: input is too brief to contain an identifiable claim and
  supporting reasoning."

If the input clearly isn't an argument but doesn't match any of the five
categories above, don't force-fit it into one. Output "No argument
detected: " followed by a one-sentence, input-specific reason written for
that input.

**Gate B — malformed spans.** Independently of Gate A, scan for any run of
roughly 10+ consecutive words that doesn't parse as language — encoding
artifacts (mojibake), stray HTML/markdown fragments, OCR garbage, or
non-linguistic symbol strings. This is an orthographic check, not a
judgment call about content.

- No such span → proceed normally to Step 0.5.
- A span exists but a real argument remains outside it → exclude only that
  span from claim extraction in Step 2, continue analyzing the rest, and
  add one line to the reveal: "Note: [n] passage(s) were excluded from this
  analysis due to corrupted or unreadable text."
- The corrupted span(s) are the entire input, nothing analyzable remains →
  treat as Gate A's "empty" case instead.

**Not handled here:** a long essay is not a sanity failure by itself — that's
a separate output-verbosity concern, not a gate condition. Don't add
length-based rejection to this step.

## Step 0.5: capture the pre-reveal guess (optional)

Before running any analysis, ask: "Before I break this down — what do you
think is the weakest point in this argument? (Or say 'skip' to go straight
to the analysis.)" Wait for a reply. If a guess is given, hold onto it for
Step 8. If the person skips, proceed directly to Step 1 with nothing to
compare against later.

## Note: Steps 1-6 run blind to Step 7's node cap

Steps 1 through 6 decide what the argument's structure *is*. Step 7 decides
how to *draw* it. That decision must go one direction only.

While extracting premises (Step 2), surfacing assumptions (Step 3), and
locking the structure (Step 6), do not consider, estimate, or optimize for
how many nodes the eventual diagram will have. The ~6-7 node cap named in
Step 7 does not exist for the purposes of any decision made in Steps 1-6 —
split a chain as far as Step 2's splitting rule requires, add a cost-side
assumption whenever it applies, and give a warrant its own node whenever
it's substantial, even if you can tell in advance the resulting list will
be too large to diagram. A large, correct locked structure that skips the
diagram is the intended outcome in that case — not a failure to be avoided
by quietly merging or under-splitting earlier.

Nothing in Steps 1-6's own text should ever reference "the cap," "the
diagram limit," or node counts — if a future edit adds such a reference,
that's a sign this note has been violated, not extended.

## Step 1: identify the conclusion(s)

State the primary claim(s) the text is arguing for, in your own words. If
the text argues for more than one distinct claim, list each separately.

This must appear explicitly in the visible reveal, independent of the
diagram — don't rely on the diagram's conclusion node to be the only place
this gets stated. If the diagram fails or is skipped, the conclusion still
has to be written out in prose.

## Step 2: extract premises

For each conclusion, extract the discrete premises offered in support. Keep
them atomic — don't bundle two separate reasons into one bullet. If Step 0
Gate B excluded a span as unreadable, don't extract anything from it — work
only from the remaining clean text.

**Split causal chains into their individual links.** A clause like "X
enables authorities to do Y, which prevents Z" reads as one continuous
idea, but it's actually multiple separate claims: X provides Y-capability,
Y-capability gets used to produce an action, and that action actually
achieves Z. Each link can hide its own gap — Step 3's chain check can only
examine links between nodes that were actually separated here; a bundled
premise hides its internal joints from that check entirely. When in doubt,
split further rather than less — an over-split premise list costs a little
redundancy, an under-split one hides a real gap where the chain-check can't
reach it.

**Check the role before boxing it.** Not every clause in the text is a
premise. A premise offers support for the conclusion. Some clauses do
something else: a caveat or scope-limit narrows what the argument is
willing to address, without supporting the conclusion (e.g. "the
environmental impact needs separate analysis, but that's not what I'm
arguing here"); a concession grants a point without endorsing it. If a
clause isn't actually offering support, don't extract it as a premise and
don't force it into the diagram later — name it in prose for what it
actually is (a caveat, a concession, a scope-limit), and treat the
assumption *behind* that move (e.g. "this bracketed question can't affect
the conclusion") as a candidate for Step 3. Don't pre-judge whether it's
worth proposing here — that's Step 3's job to hand off, and Step 4's job to
actually decide.

**Keep the concession's own text, don't just name it and move on.** A
concession is a candidate counter-response later, in Step 4 — record what
the writer actually conceded and how they framed it, verbatim or close to
it, not just "a concession was made here." If Step 4 can't see what the
concession actually said, it can't check whether it answers anything.

## Step 3: surface candidate unstated assumptions

Identify the implicit steps needed to get from the stated premises to the
conclusion that the text never actually says. Label these explicitly as
unstated assumptions — don't fold them in as if the author wrote them.

**This step only proposes candidates — it doesn't decide which ones
matter.** Step 4 is what decides whether a candidate is significant enough
to surface as a real assumption node. Don't try to judge "is this
substantial enough" here — just find the candidates and hand them to
Step 4.

**Be generous, not exhaustive.** Propose warrants a careful reader would
plausibly notice as doing real work in *this specific* argument — not every
background assumption any argument whatsoever implicitly relies on (the
uniformity of nature, the validity of logical inference, the existence of
the external world). If the same candidate would apply unchanged to nearly
any argument regardless of its subject, it's not worth proposing; Step 4
would filter it out anyway (no real objection exists to something nobody
actually contests), so there's no point spending effort generating it.

**Warrants attach to what they justify — but type by what's actually
stated.** If a premise itself appears in the text — even hedged ("I assume
X", "I think X") — it's still a stated premise, never an assumption,
regardless of how weak its underlying justification is. What's genuinely
unstated is the *warrant* — the reasoning that's supposed to make the
premise good enough to rely on (e.g. "human utility matters more, *because*
of evolutionary grounds"). Propose it as a candidate, phrased as the
positive claim the argument needs to be true (e.g. "assumes evolutionary
tendency justifies ethical permissibility"), not as skepticism about it
(e.g. "link unclear") — a candidate states what the argument is relying on,
not the reviewer's doubt about it.

**Check the cost side on evaluative conclusions.** If the conclusion is a
"should X" / net-judgment claim, check whether the argument actually
addresses what's foregone or harmed, not just what's gained. An argument
that lists only benefits toward a comparative conclusion is implicitly
assuming nothing on the other side outweighs them — propose that as a
candidate in its own right, even if no other gap is present. Treat this as
a standing check on every evaluative conclusion, not something to notice
only when it happens to stand out.

**Check every adjacent pair in the chain, not just the final join.** List
the distinct constructs the argument actually depends on, in order, from
first premise to conclusion (e.g. "measures syntax" → "quantifies
capability" → "optimizes meritocracy" → "establishes equity"). For each
adjacent pair, check whether the argument actually justifies treating them
as equivalent, or whether one term is silently standing in for a different
concept. A conclusion can look supported because the *last* link
(premise-to-conclusion) seems to connect, while an earlier link in the same
chain already swapped in a different construct with no justification —
check the whole chain by default, not just the join nearest the
conclusion.

**Comment on tone only when confidence is miscalibrated to rigor.** Don't
flag absolutist or hedged language as a standalone stylistic observation —
tone by itself isn't a finding. It's only worth mentioning when the
confidence the text expresses doesn't match the actual rigor behind it: an
argument stated with total certainty ("guarantee," "absolute," "entirely
immune") resting on nothing but unevidenced assertions is a real mismatch
worth naming, since the certainty is doing work the reasoning hasn't
earned. The reverse counts too — a claim hedged as a mere guess that turns
out to be better-supported than its own hedging suggests. When confidence
and rigor roughly match — firm language backed by real support, or an
appropriately tentative hedge on a genuinely uncertain point — tone isn't
worth a mention at all, not even in passing. The check is always about the
gap between stated confidence and actual rigor, never about register on
its own.

## Step 4: classify contestedness — and decide which candidates surface

Applies to every premise from Step 2 and every candidate assumption from
Step 3. Contestedness — how well a claim survives a steelmanned objection —
is what actually determines whether something is a weak point, not whether
it happens to cite a source: citedness isn't the same thing as
vulnerability, and this tool exists to find weak points, not to police
footnotes.

**First, once per argument** — don't re-check this per premise: does the
conclusion have an identifiable for/against split (a stance, recommendation,
or persuasive claim a reasonable person could argue the opposite side of)?

- **Yes** — frame the objection below as what a competent, good-faith
  opposing side would actually raise.
- **No** (descriptive/exploratory input — a research summary, a reflective
  piece with no clear stance) — frame it instead as the strongest reasonable
  disagreement a thoughtful, informed reader could raise. Same mechanism, no
  "sides" forced onto input that doesn't have any.

**For each premise/candidate, write out this exchange — never decide
silently:**

1. **The objection**: the single most damaging real objection available —
   drawing on any genuine position, mainstream or minority, not necessarily
   the one most commonly voiced. It must be a position someone could
   actually hold, not an invented strawman.
2. **The counter**: check first whether one of the writer's own concessions
   (kept verbatim in Step 2) already functions as a response to this
   specific objection. If one does, use the writer's own words as the
   counter and tag it `anticipated: true` — this applies whenever the
   concession genuinely addresses the objection, whether or not it fully
   defeats it; partial credit is still real credit, not just a clean win.
   Only generate a tool counter (`anticipated: false`) when no concession
   applies. This is the only place concessions get reused — don't surface
   them anywhere else in the pipeline, since their sole value here is as
   evidence of what the writer already saw coming.
3. **The verdict**: does the counter defeat the objection, does the
   objection hold its ground against the counter, or does the objection
   survive the counter and come out ahead?

**Classify from that verdict, not from a separate judgment call:**

- **No contestation** — no real objection exists to begin with.
- **Weakly contested** — an objection exists, but the counter defeats it, or
  the objection only works on a narrower reading of the premise than the
  text actually gives.
- **Contested** — the objection holds up against the counter — neither side
  clearly wins.
- **Strongly contested** — the objection survives the counter and comes out
  ahead; the original premise doesn't actually recover.

**What this verdict does, differs by node type:**

- **Premises always appear** — they exist because Step 2 found them stated
  in the text, regardless of contestedness. The verdict only sets their
  border thickness (see Step 7).
- **Candidate assumptions only become real nodes at "weakly contested" or
  above.** A candidate that comes back "no contestation" — no real objection
  survives against it at all — is dropped entirely, not shown anywhere. This
  is the actual filter that keeps Step 3's generosity in check: propose
  freely, and let this test decide what's worth a reader's attention. A
  universally-granted background assumption (uniformity of nature, validity
  of logical inference) always resolves to "no contestation" here and never
  reaches the page, without Step 3 needing to have pre-judged it.

Writing out the objection and the counter is the point, not optional
showing-of-work. This exchange is also what the icon-reveal in Step 7 shows
when clicked — if it isn't written down here, there's nothing to reveal
there.

## Step 5: check for a researchable hinge

Check whether the input contains a hinge point — an empirical factor the
argument's benefit/harm/implication depends on — where **either the arguer
has expressed explicit doubt, or Step 4 classified it as strongly
contested**, that doubt/contestation concerns something **checkable against
the historical/factual record** (not an open modeling or value judgment),
and the hinge is **load-bearing for the argument's main conclusion** — not
merely attached to a supporting illustration or example that the conclusion
doesn't actually depend on. All conditions must hold; a confident,
uncontested premise doesn't qualify, an uncertain-but-unfalsifiable judgment
call doesn't qualify, and neither does a doubted or contested fact sitting
inside an example that could be wrong without changing the conclusion at
all.

**Why strongly-contested is an addition to doubt, not a replacement:** doubt
and contestation answer different questions — doubt is about what the
arguer personally isn't sure of, contestation is about whether a real
opposing view exists. A writer can doubt something nobody would actually
contest (still qualifies, still worth checking); a confidently-stated claim
can face a real, surviving objection despite zero doubt being expressed.
Requiring either one, not swapping one for the other, means the doubt-only
cases this step already caught keep qualifying.

**A soft hedge is not automatically doubt.** Phrasing like "I think X" or
"I assume X" doesn't stop X from being a stated premise (Step 3) — the same
phrasing doesn't automatically clear this step's "explicit doubt" bar
either. Doubt-work sounds like genuine uncertainty about whether the claim
is *true* — "I'm not sure if," "I might be wrong that," "I don't know
whether X actually held" — not a conversational "I think" the arguer would
flatly defend as fact if pressed. When it's genuinely ambiguous which one a
hedge is doing, treat it as not qualifying: this step is deliberately
narrow, and a borderline case defaults to being skipped rather than
triggering research over a stylistic hedge.

**The factual-record filter already does the political/values exclusion for
free.** A live policy or values disagreement will almost always have a
strongly-contested objection available — that's what makes it contested —
so contestation alone would fire constantly on ordinary disagreements. It's
the "checkable against the historical/factual record" condition that keeps
this narrow: a values stance never passes that filter regardless of how
strongly it's contested.

If a qualifying hinge is present:

- A hinge qualifies if the arguer flagged it as uncertain, **or** if Step 4
  classified it as strongly contested — either is sufficient on its own,
  provided the record-checkable and load-bearing conditions above still
  hold. A confidently-stated premise with no live objection is still out of
  scope; this isn't open-ended error-hunting, it's checking the specific
  cases where either signal already exists.
- Confirm the hinge is load-bearing before researching it: would the main
  conclusion actually change, or at least need reframing, if this specific
  point turned out to be false? If the answer is no — the conclusion would
  hold regardless — this doesn't qualify, even if the doubt is real and the
  fact is checkable.
- Cap this step at 1-2 targeted searches.
- Fold what's found back into how the premise/conclusion gets framed in
  Step 8's reveal — note whether it changes what the argument's core
  tradeoff actually is.
- If the research surfaces a genuinely contested question rather than a
  settled fact, present the competing positions rather than picking one.
  This step checks facts against the record; it never adjudicates a live
  policy or normative debate.
- **If a search incidentally surfaces information about a different,
  non-qualifying candidate** (one that already failed the conditions
  above), don't fold it into the reveal as if it were an earned Step 5
  finding — it wasn't produced by a deliberate, in-scope search. Don't
  suppress it either: mention it briefly, separately, as an aside, so the
  reader isn't kept in the dark about something directly relevant that
  turned up. Never let spillover trigger a further search of its own — the
  1-2 search cap and the load-bearing gate apply only to the hinge that was
  deliberately investigated.

If no such hinge exists, skip this step entirely — most runs won't have one.

## Step 6: lock and state the premise/conclusion map

Before any diagramming, write out the complete, final structure as visible
text: every premise (with its contestedness level), every assumption that
Step 4 surfaced as weakly-contested-or-above (with its warrant if one is
attached, and its contestedness level — candidates Step 4 dropped at "no
contestation" don't appear here at all), and the conclusion(s). This is not
a draft — once written, it is the fixed structure that Step 7 diagrams.
Step 7 is required to render exactly this list: no new premises invented at
diagram time, no retyping a premise as an assumption or vice versa, no
changing a contestedness level after the fact, no reviving a dropped
candidate, no wiring relationship that wasn't already implied by what's
stated here. If the structure feels off once reaching the diagram, that's a
signal to come back and revise this list explicitly, not to quietly patch
it while drawing.

## Step 7: diagram the locked map

Render exactly the structure fixed in Step 6 — this step draws it, it does
not decide it. Load the diagram module (`visualize:read_me` with `modules:
["diagram"]`), then call `visualize:show_widget`. Do not narrate the
diagram in prose first — the widget itself is the deliverable.

- One box per premise, one per conclusion (or sub-conclusion, if the text
  has more than one layer of reasoning).
- Color nodes by type (premise / assumption / conclusion), same convention
  as `/inspect` — a secondary cue, not the primary type signal.
- Unstated assumptions: dashed-border boxes, premises/conclusions:
  solid-border boxes — same convention as `/inspect`. This is the primary
  type signal and border *style* never changes regardless of contestedness.
- Border *thickness* encodes contestedness, independent of style, same
  4-point scale for every node type: no contestation 0.5px, weakly
  contested 1px, contested 1.5px, strongly contested 2.5px. A premise can
  land anywhere on this scale, including 0.5px, since premises always exist
  once Step 2 finds them. An assumption node only ever reaches the diagram
  at 1px or above — a candidate that resolved to "no contestation" in Step 4
  never became a node, so 0.5px dashed never actually occurs.
- Arrows from premises into the conclusion they support.

**After the diagram (or in its place if Step 7 is skipped), render the
contestedness icon-reveal list.** One row per premise and per surfaced
assumption node — dropped candidates (Step 4's "no contestation" verdict)
don't get a row, since they were never shown as nodes either. Each row gets
a small info icon; clicking it reveals the objection → counter → verdict
exchange written in Step 4 for that node. When the counter came from one of
the writer's own concessions, label it as such in the reveal — e.g.
"Counter: [the concession, in the writer's own words] — anticipated by the
writer" rather than presenting it identically to a tool-generated counter.
This list is independent of whether the diagram renders — it must appear
either way, since its purpose is showing the reasoning that produced each
contestedness label, not decorating the diagram.

**If the `visualize:` tool isn't available in the current environment** (the
module fails to load, or `show_widget` errors), don't retry or block on
it — fall back to a plain-text structure map instead: an indented tree,
conclusion at the root, each premise/assumption labeled with its
contestedness level inline, followed by the objection → counter → verdict
exchange for each. This fallback is not a downgrade in the analysis, only
in presentation — every element from Step 6 must still be represented.

**When to skip the diagram**: if the input has no real premise-to-conclusion
structure (a single assertion, a mood, description with no argument), say so
and skip it rather than inventing structure that isn't there. The same
applies if the Step 6 locked structure has too many nodes to track visually
(roughly more than 6-7 premises/assumptions/conclusions combined) — "skip"
means draw no diagram at all and rely on the prose list Step 6 already
produced. It never means simplifying, merging, or dropping nodes to fit the
diagram under the cap; the locked list from Step 6 is never edited at
diagram time, whether or not a diagram gets drawn from it. A cluttered,
hard-to-read diagram is worse than no diagram.

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
onward to the conclusion as if it still contributes. Ask: does every arrow
represent an inference that's actually being made, or did it just complete
the picture?

## Step 8: reveal and compare

Show the full analysis (Steps 1-7's output). Then add two further sections,
in this order, before comparing against any pre-reveal guess:

**Likely rebuttals.** Pull every premise/assumption node Step 4 classified
"contested" or "strongly contested." For each: state the objection plainly,
state the existing response (and whether it's writer-anticipated, per Step
4), and state where the exchange landed — holds up, wins if pushed, or
still open. This reuses Step 4's own exchanges; nothing gets generated
fresh here. Omit this section entirely if no node clears "contested" —
don't force it to appear over a clean argument.

**Unaddressed considerations.** Independent of any specific premise or
assumption, think as a critical, informed observer would: is there a real
right, value, framework, or consideration the argument never engages with
at all — not an attack on something it said, but something it never
touched? Use the same for/against framing already established in Step 4
(a stance-like conclusion gets a devil's-advocate opposing-side framing; a
descriptive/exploratory conclusion gets the "thoughtful, informed reader"
framing) regardless of formal debate structure.

- Only surface candidates a critical, competent observer would actually
  raise unprompted — not any conceivable tangential value. Typically 1-3 per
  run; don't pad to hit a number, and don't manufacture one if nothing
  genuinely clears the bar.
- Check each candidate against the rebuttals list above and drop anything
  that's really just restating an already-covered contested premise or
  assumption rather than a genuinely separate, unaddressed consideration.
- State the consideration plainly and explain concretely what about it the
  argument leaves unaddressed. No severity or fatality grading — this
  surfaces the gap, it doesn't rank how damaging it is or declare a winner
  on whatever values question sits underneath it.
- Omit this section entirely if no genuine candidate exists.

If a pre-reveal guess was given in Step 0.5, briefly note how it compares to
what the full reveal (including both sections above) found — did it
anticipate the main gaps, or does the analysis add something beyond it. If
Step 0.5 was skipped, just present the analysis without this comparison.

If Step 5 ran, present its finding as its own distinct point in the
write-up — a factual correction is different in kind from a
logical/structural gap, so keep it visibly separate rather than folding it
into the assumption list or the comparison above.

## Formatting

Keep prose tight; let the diagram carry the structural weight rather than
re-describing every box in text. Use clear, plain-language section headers
(never numbered "Step N" labels — see the presentation note above) so the
reveal reads as one coherent analysis, not several disconnected sections.

## Editing this skill

Whenever this file's logic changes, bump the `Version` line above.
