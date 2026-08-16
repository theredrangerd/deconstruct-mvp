# deconstruct-mvp-v1

An early-stage **Claude skill** that breaks an argument — a debate case, essay draft,
research excerpt, or other piece of persuasive writing — into its actual logical
parts: conclusion, premises, contestedness, and the unstated assumptions the
argument quietly depends on. It optionally checks one flagged, load-bearing
factual claim against the record, and diagrams the resulting structure.

This is a **draft, not a finished product**. It's part of a personal project on
improving human reasoning with AI assistance, aimed at people who need to argue
well under scrutiny — debaters, MUN delegates, aspiring lawyers, and anyone
picking apart their own writing before someone else does it for them.

I'm posting it publicly to get outside feedback: does it actually surface real
gaps in an argument, or does it miss things / hallucinate structure / feel
tedious to use? Bug reports, confusing-instruction reports, and "here's an input
that broke it" reports are all useful.

## What it does

Given a piece of argumentative text, it:

1. Checks that there's actually an argument to deconstruct — rejects input with
   no identifiable claim-plus-support (a list of facts, a narrative, a one-liner,
   pure sentiment), and excludes any corrupted/unreadable text span rather than
   choking on it or silently guessing.
2. Optionally asks you to guess the argument's weakest point before revealing
   anything (so you can check your own instincts against the analysis).
3. States the conclusion(s) being argued for.
4. Extracts the premises offered in support, kept atomic (causal chains get
   split into individual links).
5. Surfaces unstated assumptions the argument depends on but never states —
   including checking every link in a reasoning chain, not just the final one,
   and checking whether an evaluative conclusion ("we should do X") actually
   weighs costs, not just benefits.
6. Classifies every premise and surfaced assumption by **contestedness**: for
   each one, it writes out the strongest real objection, checks whether the
   writer already anticipated it with their own concession, and reaches a
   verdict — no contestation, weakly contested, contested, or strongly
   contested. Assumptions that don't survive any real objection are dropped
   entirely rather than padding the output.
7. If the arguer flagged real doubt about a specific, checkable, load-bearing
   fact — or if the contestedness check itself turned up a claim that comes out
   strongly contested — does a small amount of targeted research on just that
   point.
8. Diagrams the whole structure (premises, assumptions, conclusion) as a visual
   map, with border thickness showing each node's contestedness.
9. Surfaces the likely rebuttals a critical reader would raise, plus any real
   consideration the argument never addresses at all — not just attacks on
   what it said, but things it never touched.
10. If you made a guess in step 2, compares it to what the full analysis
    actually found.

## Installing

**claude.ai (primary, recommended):**
1. Download `deconstruct-mvp-v1/SKILL.md` from this repo (or clone the repo).
2. Zip the `deconstruct-mvp-v1/` folder so the zip contains
   `deconstruct-mvp-v1/SKILL.md` directly (not a doubly-nested folder):
   ```
   zip -r deconstruct-mvp-v1.zip deconstruct-mvp-v1/SKILL.md
   ```
3. In claude.ai, go to Settings → Capabilities → Skills (or the skill upload
   option in your workspace) and upload the zip as a custom skill.
4. Start a new chat and type `/deconstruct-mvp-v1` followed by the text you
   want broken down.

**Claude Code:**
Drop the `deconstruct-mvp-v1/` folder into your skills directory:
```
cp -r deconstruct-mvp-v1 ~/.claude/skills/
```

## Using it

Once installed, start a message with `/deconstruct-mvp-v1` followed by the text
you want broken down — a debate case, an essay paragraph, an op-ed excerpt,
your own draft. Example:

```
/deconstruct-mvp-v1 We should ban single-use plastics because they take
hundreds of years to decompose and are choking marine wildlife. Countries
that have already banned them have seen measurable drops in ocean plastic.
```

## Known limitations (read before reporting these as bugs)

- The diagram step uses claude.ai's native `visualize:` widget. If that tool
  isn't available in your environment, it falls back to a plain-text
  structure map instead — that's expected behavior, not a failure, and the
  underlying analysis (premises, assumptions, contestedness, exchanges) is
  unaffected either way.
- The "no argument detected" gate is a structural check, not a quality
  judgment — it fires on input with no identifiable claim-plus-support at
  all, and won't run the full analysis just because you insist there's an
  implicit argument in there. Give it the conclusion explicitly and it'll go.
- The fact-check step only fires on a narrow kind of hinge (explicit doubt,
  or a strongly-contested claim, that's also checkable and load-bearing on
  the main conclusion) and is capped at 1-2 searches — it's intentionally
  conservative, not a general fact-checker.
- This has been adversarially tested against a range of scenarios by its
  author, but not by anyone else yet — that's the point of this post.

## Feedback

Either works: open an issue on this repo, or send me a DM on Reddit —
u/PosteriorMotives24. Useful feedback looks like: a specific input you ran,
what the skill produced, and what you expected instead.

## License

MIT — see [LICENSE](LICENSE). Use, fork, and modify freely.
