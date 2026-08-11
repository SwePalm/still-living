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
Verdict: (human, 2026-08-11) Rejected — not for a specific fault, but as not good
enough to keep on the wall. The bar is Stefan's wife, and the piece was not clearing
it. That verdict is what licensed a bold move here rather than a drift.

## generation 5 — the ink learns where the light is (2026-08-11)

Human-directed, implementing `proposals/001-cumulus-not-fog.md` — Stefan's own idea
that the field was "very gas-light" and should read more like cumulus than fog. He
gave the go in-session after generation 4 was rejected. That go also covers the two
things here that are otherwise out of free scope: a change to the shader's character,
and a new `form` block in genome.json (CONSTITUTION.md, "requires a human yes").

The proposal had two halves. **One shipped, one failed its own gate.** The failure is
the more useful record, so it goes first.

**Rejected: billow noise.** Rectifying each octave (`mix(n, abs(n*2-1), solidity)`)
does exactly what the proposal claimed — a crease at every zero crossing, and the
silhouette gains cauliflower edges. In isolation it looked more cloud-like. But
measured against generation 4 in a tight A/B on the same frame, changing only the
dial, it destroyed large-form luminance contrast every time:

| clear day, same moment | mean luminance | luminance SD |
|---|---|---|
| fog (generation 4) | 0.186 | 0.097 |
| shading only | 0.207 | **0.112** |
| shading + billow 0.25 | 0.187 | 0.075 |
| shading + billow 0.55 | 0.171 | 0.061 |

Root cause: smooth value noise concentrates near 0.5, so `abs(2n-1)` concentrates
near 0 — each octave's contribution shrinks and the sum's variance collapses. The
field gains fine texture and loses its big light/dark masses. That is precisely the
trade verdict 1 forbade: shapes moving out of luminance, where cast compression eats
them. It would have looked good here and turned to soup on the 65". Removed entirely,
leaving no dead `solidity` knob in the genome. If billow is ever revisited it needs
per-octave variance renormalisation, not a mix.

**Shipped: directional self-shadowing.** The density field is sampled a second time,
one step toward a light direction. Denser there means this pixel sits in another
fold's shadow; thinner means it is the face turned into the light.
`col *= 1.0 - sh * (occl - face * 0.75)`, with `sh` from the new `form.shade` (0.50).
The light follows the sun the piece already tracks — overhead by day, raking low at
night, swinging across the 80-minute dusk ramp — so its sense of time of day now
shows up in the *form*, not only the palette. Cloud cover damps it
(`* (1 - u_soft * 0.45)`): a clear day has a lit body, an overcast one goes flat.
One more coupling to the world.

The lit-face coefficient matters more than it looks. At the first value (0.45) the
shadow outweighed the highlight and shading *lowered* luminance SD to 0.082 — form
appearing, structure disappearing. At 0.75 the two balance and SD rises to 0.112,
above the fog it replaced. So this mutation moves the way verdict 1 demanded: more of
the composition carried in luminance, not less.

Cost: 6 fbm taps per pixel become 8. Benchmarked by driving draws manually and
syncing with a pixel read (`gl.finish()` is a no-op in the browser and reports
nonsense — 43,000 fps): 0.123 → 0.150 ms/frame at 1080p, **+22%**. The governor
absorbs that at roughly 0.9× the previous linear render scale, so 60fps holds and the
image is marginally softer. Real TV fps is unverified from here — worth watching.

Gate, five states plus both fallback checks: shader compiles, no console errors.
Clear day 0.152 (RGB 55/31/38, red-dominant), overcast night 0.171 (38/44/55), starry
night 0.175 (35/46/64), snowfall 0.247 (60/62/77), storm 0.156 (38/39/49) — all
inside 0.04–0.85, no near-black, near-white at most 0.04%, all five clearly distinct.
`DEFAULT_GENOME` verified equal to genome.json; served alone with genome.json removed,
clear day renders the same red-over-slate with the same shading, so cold starts and
offline show the current organism.

Honest critique against the identity clause: still abstract, calm, continuous, still
the ink. It gained depth without gaining drama — motion, breath and palette are
untouched, and the shading is a multiply that cannot flash. The thing to watch is the
night states, where lifting the lit faces pulls a slightly khaki cast out of the night
palette's warm side. It reads as dusty rather than muddy at this strength, but if a
future generation raises `form.shade` it will get worse before it gets better.

**Constitution gap, for the human.** `form.shade` has no range in CONSTITUTION.md,
which the agent may never edit, so future runs have no sanctioned bound. Suggested
line under free scope's genome ranges: `form: shade 0–0.6`.

Verdict:
