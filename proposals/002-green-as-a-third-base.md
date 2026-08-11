# proposal 002 — green as a third base colour

**Status:** human-originated (Stefan, 2026-08-11).
**Scope:** *in* free scope — this needs no yes. It is pure `palette.day.d` and
`palette.day.b` movement, both inside the constitution's ranges. A future run can
simply do it as its one mutation. Written down here because it carries a trap that
would otherwise get rediscovered the hard way.

## Motivation

Stefan: *"what if we would add green as a third base color?"*

## Why there is currently no green

The palette is `a + b * cos(2π(t + d))` per channel. Each channel is one sinusoid, so
a channel's hue contribution peaks where `t = -d`. Today's day genome has
`d = [0.00, 0.15, 0.18]`:

| channel | phase | peaks at t |
|---|---|---|
| R | 0.00 | 0.00 |
| G | 0.15 | 0.85 |
| B | 0.18 | 0.82 |

Green and blue peak 0.03 apart. They rise and fall together, so green never appears as
itself — it only ever tints the blue. That is the whole reason the piece is a two-colour
organism. Green is not missing by choice; it is missing because its phase is welded to
blue's. **Separating `d.g` from `d.b` is the entire change.**

## The trap

Green can be separated in two directions, and one of them is forbidden.

Move `d.g` toward 0.5 and green peaks in the middle of the sweep, on the *warm* side —
between red's peak and blue's. Sampling `d = [0.00, 0.50, 0.18]` at t = 0.25 gives
`(0.54, 0.40, 0.17)`: **amber**. That is the ochre the human retired in verdict 1,
reintroduced through the back door. Any naive "add green" lands here, because 0.5 is
the obvious phase to reach for.

Move `d.g` the other way — to roughly 0.30, just past blue — and green emerges on the
*cool* side, as teal, with no warm midtone anywhere in the sweep.

## Sketch

A starting point worth trying, not a final answer:

```
day: { a: [0.54, 0.40, 0.46], b: [0.42, 0.24, 0.32], d: [0.00, 0.30, 0.18] }
```

`b.g` drops 0.28 → 0.24 to keep the green muted — sage and teal rather than a pure
green, which would fight the calm. Sampling the sweep:

| t | rgb | reads as |
|---|---|---|
| 0.00 | 0.96, 0.33, 0.60 | pink |
| 0.25 | 0.54, 0.17, 0.17 | deep red — unchanged |
| 0.50 | 0.12, 0.47, 0.32 | green-teal — the new band |
| 0.70 | 0.41, 0.64, 0.69 | pale cyan |
| 0.82 | 0.72, 0.57, 0.78 | lilac |

Red and blue both survive as anchors, per verdicts 2 and 3. Green arrives as the
transition between them rather than as a third stripe competing with them.

## Risks

- **Rainbow.** Three bases across one sweep can tip from "living" into "spectrum".
  Keeping `b.g` low and green confined to the red→blue crossing is what prevents that.
  If it reads as a gradient demo, revert.
- **Season.** Green is a spring and high-summer colour here. Introducing it in late
  August is slightly against the year — it may be better held until spring, or brought
  in first on the night palette, where an olive cast already exists in the current
  states and would go almost unnoticed.

## Note in favour

Green is the largest term in luminance (0.587 of it). A green band is therefore
*luminance-strong* by construction, which is exactly what verdict 1 asked future
palettes for. Of the three bases, green is the one the television will render best.
