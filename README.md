# deconstruct-mvp-v1

An early-stage **Claude skill** that breaks an argument — a debate case, essay draft,
research excerpt, or other piece of persuasive writing — into its actual logical
parts: conclusion, premises, evidence-vs-assertion, and the unstated assumptions
the argument quietly depends on. It optionally checks one flagged, load-bearing
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

1. Optionally asks you to guess the argument's weakest point before revealing anything (so you can check your own instincts against the analysis).
2. States the conclusion(s) being argued for.
3. Extracts the premises offered in support, kept atomic (causal chains get split into individual links).
4. Classifies each premise as evidence-backed or merely asserted.
5. Surfaces unstated assumptions the argument depends on but never states — including checking every link in a reasoning chain, not just the final one, and checking whether an evaluative conclusion ("we should do X") actually weighs costs, not just benefits.
6. If the arguer flagged real doubt about a specific, checkable, load-bearing fact, does a small amount of targeted research on just that point.
7. Diagrams the whole structure (premises, assumptions, conclusion) as a visual map.
8. If you made a guess in step 1, compares it to what the analysis actually found.

## Installing

**Claude Code:**
Drop the `deconstruct-mvp-v1/` folder into your skills directory:
```
cp -r deconstruct-mvp-v1 ~/.claude/skills/
```

**claude.ai:**
Zip the `deconstruct-mvp-v1/` folder (so the zip contains `deconstruct-mvp-v1/SKILL.md`,
not a doubly-nested folder) and upload it as a custom skill in claude.ai's skill settings.
```
zip -r deconstruct-mvp-v1.zip deconstruct-mvp-v1/SKILL.md
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

- The diagram step depends on your Claude environment supporting inline
  artifacts/canvases — if that's unavailable, it falls back to a text-only
  breakdown, which is expected behavior, not a failure.
- The fact-check step only fires on a narrow kind of hinge (explicit doubt +
  checkable + load-bearing on the main conclusion) and is capped at 1-2
  searches — it's intentionally conservative, not a general fact-checker.
- This has been adversarially tested against roughly a dozen scenarios by
  its author, but not by anyone else yet — that's the point of this post.

## Feedback

Open an issue on this repo, or reply wherever you found the link. Useful
feedback looks like: a specific input you ran, what the skill produced, and
what you expected instead.

## License

MIT — see [LICENSE](LICENSE). Use, fork, and modify freely.
