# Lineage

Newest generations at the bottom. Humans write verdicts under a generation's entry;
verdicts are binding taste for all future mutations (see CONSTITUTION.md).

## generation 0 — founding (2026-07-04)

Body and genome separated; no visible change. The ink species as born in the first
sessions: domain-warped fbm flow field, three-oscillator breath (7.3/11.9/28.6s),
perpetual two-layer expansion, ochre/slate day palette, ink/teal night palette,
weather-driven mood via Open-Meteo, snow and starry-night behaviors.
Verdict:

## generation 1 — high-summer gold (2026-07-04)

No verdicts yet in the lineage, so this drift takes its cue from the season: it's
early July in Stockholm, near-perpetual daylight. Warmed the day palette only —
`palette.day.a` from [0.46, 0.42, 0.38] to [0.48, 0.43, 0.36], `b` from
[0.38, 0.32, 0.28] to [0.40, 0.33, 0.26], `d` from [0.02, 0.12, 0.28] to
[0.02, 0.11, 0.25] — a small shift toward amber/gold and away from slate, leaving
night, stars, snow, and storm palettes untouched. All within the a/b (0–1) and
d-phase (0–1) ranges. Screenshots at the five forced-mood URLs: clear day now
reads as warm ochre-gold rather than muted ochre-slate; overcast night, starry
night, snowfall, and storm are visually unchanged in character and clearly
distinct from each other and from day. No console errors, no near-black or
near-white frames. Against the identity clause: still abstract, calm, continuous,
still the same organism — this is a mood tint, not a new species.
Verdict: (human, 2026-07-10) On the 65" TV the orange/ochre day palette loses its
shapes — "looks like a soup". Diagnosis: its forms were drawn in chroma, not
luminance, and TV/cast compression discards chroma detail. Retired in favor of a
red base (generation 3). Future palettes must carry their shapes in luminance.

## generation 2 — white nights (2026-07-10)

Generation 1 warmed the days; this one takes the other half of the Nordic high
summer: the nights. Stockholm in July never gets truly dark, so the night palette
lifts slightly toward a pale luminous blue — `palette.night.a` from
[0.09, 0.11, 0.15] to [0.11, 0.13, 0.18], `b` from [0.11, 0.15, 0.21] to
[0.12, 0.16, 0.22], `d` from [0.55, 0.62, 0.72] to [0.56, 0.63, 0.71]. Day,
tints, motion, stars, snow untouched. All values within a/b (0–1) and d-phase
(0–1) ranges. Screenshots at the five forced-mood URLs: overcast night now reads
as a pale grey-blue dusk rather than deep ink; starry night reads as a real
summer night — faint stars in a luminous sky; clear day, snowfall, and storm
unchanged in character; all five clearly distinct. No console errors, no
near-black or near-white frames. Identity clause: still the same calm organism —
its nights just learned what latitude they live at. (Run supervised in-session
because the 07:30 scheduled run stalled on a tool-permission prompt; see below.)
Verdict: (human, 2026-07-10) The blue palette reads beautifully on the TV, even
through cast compression. Keep blues as an anchor of the piece.

## generation 3 — red looks better on television (2026-07-10)

Human-directed generation, from Stefan's report after casting to his 65" TV: the
orange day palette read as shapeless "soup" while the blues stayed clear. Root
cause: video encoders subsample chroma, so shapes drawn in color-contrast melt
while shapes drawn in luminance survive. Two changes: (1) the day palette moves
from ochre/amber to a red base — `palette.day` a [0.54, 0.40, 0.46],
b [0.42, 0.28, 0.32], d [0.00, 0.15, 0.18] — spanning pale pink highlights, deep
red body, and purple where it meets the blue; warmTint rosied to
[1.08, 0.97, 0.98] so warm days don't drag it back toward orange. (2)
`look.contrastSoft` raised 0.95 → 1.08 so fully overcast days keep structure
instead of flattening. Validated: clear day (pink-lit red over blue), dusk at
night=0.5 (plum/purple blend, faint stars), 100%-cloud day (shapes clearly
present), night states untouched. No console errors.
Verdict: (human, 2026-07-10) "The red looks much better than orange." Confirmed on
the TV. The red base stays.
