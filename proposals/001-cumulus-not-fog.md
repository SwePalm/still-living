# proposal 001 — cumulus, not fog

**Status:** human-originated (Stefan, 2026-08-11), awaiting an explicit go.
**Scope:** out of free scope. Changes the shader's character, not just its values.

## Motivation

Stefan: *"today the clouds are very gas-light, what if we made them a touch more
solid. Maybe not as solid as paint... but more like cumulus clouds than fog."*

He is describing something real about the current field. `fbm()` sums five octaves of
value noise at halving amplitude. That construction is smooth everywhere — the density
gradient never steepens, so nothing in the image ever acquires an edge. It reads as
fog because it *is* fog: a continuous density gradient with no surface anywhere in it.

Cumulus reads as solid for two reasons, and only one of them is about the noise.

## Sketch

**1. Billow noise for the puffy silhouette.** Rectify each octave before summing —
`a * abs(noise(p) * 2.0 - 1.0)` instead of `a * noise(p)`. The absolute value creates a
crease at every zero crossing, and stacked across octaves those creases become the
cauliflower edge that says "cumulus". One changed line in `fbm()`. Cheap, and it alone
gets maybe half the way there.

**2. A light direction — this is the one that actually matters.** Solidity is not a
silhouette property, it is a *shading* property. A shape looks solid when it is lit
from somewhere and self-shadows away from that light. Sample the density field a short
step toward a light vector and darken the current pixel in proportion to what is found
there:

```
float lit = fbm(uv * s1 + w * r * u_warpPush + lightDir * 0.12);
col *= 1.0 - shadeAmount * clamp(lit - f, 0.0, 1.0);
```

That is a cheap fake — one extra fbm tap, no raymarching — but it is the difference
between a puffy stain and a body with volume. The light direction should come from the
sun the piece already tracks, so the clouds are lit from where the real sun is. That
would be a genuinely beautiful coupling: the artwork's existing sense of time of day
would start showing up in the *form*, not only the palette.

**3. Keep the softness.** "Not as solid as paint" is the constraint. Both effects want
a genome-driven amount (`solidity` 0…1, `shade` 0…1) so the strength is a mutable value
and future generations can dial it rather than re-editing the shader.

## Risks

- **Figuration.** The constitution says never figurative. Billow noise plus directional
  shading is exactly the recipe for *recognisable* clouds. That crosses from abstract
  into representational, and it is the real danger in this idea — a companion that
  looks like a sky is a different artwork. Mitigation: keep the domain warp strong, so
  the puffs stay in motion and never settle into a readable sky. Worth validating
  hard against the identity clause.
- **Calm.** Sharper edges are busier. The piece is a living-room companion; "when in
  doubt, quieter" is binding. Solidity should probably arrive well below where it
  first looks impressive in a screenshot.
- **Cost.** One extra fbm tap is five more noise samples per pixel. The adaptive
  resolution governor will absorb it, but it must be checked on the TV, not just here.

## Note in favour

This one may help on the television. Verdict 1 established that the piece must carry
its shapes in luminance, because cast compression discards chroma. Directional shading
draws form in luminance *by construction* — it is exactly the kind of structure that
survives the encoder. This idea and that verdict point the same way.
