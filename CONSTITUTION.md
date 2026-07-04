# The constitution of still-living

This artwork evolves autonomously. A human is *on* the loop, not *in* it: within the
scope defined here the evolving agent has full freedom and asks no one. At the edge of
scope it must write a proposal and wait for a human yes. This document is the boundary.

**The agent may never modify this file.** Only a human commits changes to the
constitution. If the agent believes the constitution should change, that is itself a
proposal.

## Identity (what makes it this artwork — preserve at all costs)

- Abstract, calm, continuous. Never figurative, never text on screen, never UI.
- Alive at three timescales: breath (seconds), mood (hours, from real weather and sun),
  evolution (days, through this loop).
- The world remains its sensory organ: weather, wind, precipitation, the real local
  sunrise and sunset. Meaningful variation, not arbitrary randomness.
- It is a companion in a living room, not a screensaver demo. When in doubt: quieter.

## Hard physiological limits (never crossed, in or out of scope)

- **No strobing or flashing.** Full-field luminance must never oscillate faster than
  the slowest breath oscillator permits (period ≥ 4s). This protects photosensitive
  viewers and is non-negotiable.
- Must hold ~60fps on weak TV browsers; the adaptive resolution governor stays.
- Must survive offline: the fallback genome compiled into the body must always render.
- A failed or missing genome.json must never black out the screen.

## Free scope — mutate without asking

One mutation per run. Every mutation must pass the validation gate (see EVOLVE.md)
and be recorded in MUTATIONS.md. Within that discipline, full freedom over:

- **genome.json values**, within these ranges:
  - breath: 2–4 oscillators' values; period 4–60s; sum of amps ≤ 0.5
  - palette: a, b components 0–1; d phases 0–1; tints 0.85–1.15 per channel
  - motion: flowRate 0.01–0.08; warpBase 0.6–1.1; warpBreath 0.05–0.3;
    warpPush 0.8–2.2; expansionRate 0.004–0.03; expansionRange 0.3–0.9
  - look: contrasts 0.7–2.0; vignette 0.15–0.4; grain 0–0.04
  - stars: threshold 0.90–0.98; size 0.05–0.20
  - snow: wash 0.2–0.7
  - weather mapping: flowBase 0.3–0.8; flowWind 0.5–1.2; warpBaseMood 0.5–1.0;
    warpWind 0.5–1.2; warmthPivot 0–15; warmthScale 15–35
- **Subtle refinements to the ink species' shader** that preserve identity: texture
  quality, banding fixes, refinement of an existing behavior (stars, snow, rain,
  expansion). The piece after the change must still be recognizably the same organism.

## Requires a human yes — proposal first

- A new species, or retiring one.
- Changes to the body: the genome loader, weather fetching, the fps governor,
  fullscreen/HUD behavior, deployment, this repo's structure.
- Changing the *shape* of genome.json (new fields, removed fields, changed meaning).
- New inputs: additional APIs, sensors, audio, interactivity.
- Anything that puts text, figures, or notification-like elements on screen.
- Any change to CONSTITUTION.md.

## How to ask

Write `proposals/NNN-short-slug.md` (motivation, sketch of the change, risks), open a
GitHub issue titled `proposal: <slug>`, commit the proposal file, and stop. Do not
implement. Silence is a no. A human merges or answers on the issue when they choose.

## Selection

The human may revert any generation at any time — that is an extinction, not an error.
Humans record verdicts in MUTATIONS.md under a generation's entry. Verdicts are
binding taste: the agent must read them before mutating and steer future mutations
accordingly. Git history is the fossil record; nothing is ever truly lost.
