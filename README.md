# design-constraints

A Claude skill that designs UI by handing the model **explicit constraints**
instead of letting it guess.

## The problem

A model asked to invent an interface from nothing produces the average of
everything it has seen. That average has a recognisable look — evenly rounded
cards, four font weights, a gradient hero, padding that drifts ten pixels between
siblings. Asking for "better taste" does not fix it, because taste is not the
missing input. **Decisions are.**

## The approach

Lock the grid, the radii, the padding and the type to a small set of legal
values. The model stops guessing about spacing and starts working on the actual
problem.

```
Base grid       8px
Outer padding   24px
Outer radius    24px
Inner radius    outer − inset          (24px frame, 8px inset → 16px child)
Families        2 max
Weights         2-3 max per component
Sizes           2-3 max per component
```

Then iterate **component by component**, not screen by screen — a whole screen
generated at once gives you twenty decisions to review simultaneously, so you
review none of them.

## Install

Any one of these. All three install the same skill.

**Agent Skills CLI** — one line, no Claude Code required:

```bash
npx skills add adamperlis/design-constraints
```

**Claude Code plugin** — versioned, updates with `claude plugin update`:

```bash
claude plugin marketplace add adamperlis/design-constraints
claude plugin install design-constraints@design-constraints
```

**Manual** — clone straight into your skills directory:

```bash
git clone https://github.com/adamperlis/design-constraints.git \
  ~/.claude/skills/design-constraints
```

Then invoke with `/design-constraints`, or let it trigger on its own when you ask
to build or restyle an interface.

## What's in it

The constraints above, plus the part that is harder to come by: **the failure
modes that show up when you actually apply them.** Nested padding that silently
doubles an inset. Tokens that compile but never resolve, leaving the palette
decorative. Hover states that animate without identifying the target. Effects
that composite over and swallow a 16px glyph. Canvas animations that render
nothing because the tab was backgrounded and `requestAnimationFrame` never fired.

Each one is specific, checkable, and cost someone real time to find.

## Credit

The framing — explicit constraints over model guesswork, iterating component by
component — follows [Marvin Schwaibold](https://x.com/marvinschwaibold)'s account
of the approach. The specific values, failure modes and checklist come from
applying it in production.

## License

MIT
