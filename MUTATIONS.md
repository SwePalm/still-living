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

## generation 4 — the fallback was still yellow (2026-08-11)

Not a drift. A repair, human-directed mid-run: Stefan reported that the piece had
"regressed back to blue and yellow" — the exact ochre-and-blue soup that verdict 1
retired and generation 3 replaced with red.

Root cause, and it was never in genome.json: the genome exists **twice**. Once in
genome.json, which has carried the approved red palette since generation 3, and once
as `DEFAULT_GENOME` compiled into index.html — and that copy was never updated. It
still held the generation-0 ochre day palette `a [0.46, 0.42, 0.38]`,
`b [0.38, 0.32, 0.28]`, `d [0.02, 0.12, 0.28]`, the yellow `warmTint
[1.07, 1.00, 0.92]`, and `contrastSoft 0.95`.

That copy is not only the offline safety net. `G` is initialised from it on *every*
load and lerps toward the fetched genome over `GENOME_TAU_S = 60` — so every single
cold start opened in the retired yellow and took minutes to become red, and any load
where genome.json was unreachable stayed yellow forever. On a TV that reloads
whenever the cast restarts, that is most of what the room actually saw. The evolving
agent had been validating screenshots taken seconds after load and reading the stale
fallback as if it were the current genome — the loop was grading the wrong organism.

Fixed by syncing `DEFAULT_GENOME` to genome.json exactly: red day palette, rosy
warmTint, night palette and `contrastSoft 1.08` from generations 2–3, `generation: 4`.
No shader code and no loader logic touched; genome.json itself is unchanged from
generation 3 apart from the generation number. Editing the body is normally out of
free scope per CONSTITUTION.md — this had the human's explicit yes, given in-session.

EVOLVE.md updated in the same commit so this cannot recur: a "two-copy rule" in the
apply step, and two new gates in the validation step — a `node` one-liner that fails
if `DEFAULT_GENOME` and genome.json disagree, and an offline render test (serve
index.html alone, with no genome.json beside it, and look at what appears). Also a
note in the remember step that a retired look must be purged from both copies, and
an instruction to screenshot clear-day both immediately on load and after the genome
settles — if the palette differs between the two, the fallback is stale.

Screenshots: with genome.json removed entirely, clear day now renders deep red over
slate-blue — the approved look — where before the fix the same test produced the
rejected orange-and-blue. Served normally, clear day is red from the first frame
(mean luminance 0.156, mean RGB 59/31/37, red-dominant). Overcast night, starry
night, snowfall and storm all unchanged in character and clearly distinct; storm
reads slate with muted red beneath, snow keeps its pale hush over a faint red body.
No console errors in any state. All five within the 0.04–0.85 luminance gate
(0.156–0.265). Identity clause: unchanged organism — this run took nothing away, it
just stopped an ancestor from overwriting its descendant.

Deferred to a future generation: a late-summer night drift, prepared and validated
earlier this run before the repair took priority — `palette.night.a` down to
[0.10, 0.12, 0.17] with `b` up to [0.13, 0.17, 0.23], stars threshold 0.94 → 0.935
and size 0.11 → 0.115, for the real darkness returning to Stockholm in August as the
white nights end. It passed the gate on its own (starry night mean luminance 0.157
vs 0.173 before, blue-dominant 27/42/66, stars slightly more present). Reverted
unapplied so this generation stays a single clean repair.
Verdict:
