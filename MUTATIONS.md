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

## generation 6 — the dark comes back (2026-08-13)

A pure seasonal drift, and the one generation 4 explicitly deferred: *"a late-summer
night drift … for the real darkness returning to Stockholm in August as the white
nights end."* Generation 2 lifted the night palette for the white nights of July.
It is mid-August now; that lift is out of season, so the nights come back down.

`palette.night.a` [0.11, 0.13, 0.18] → [0.09, 0.11, 0.16] and `b`
[0.12, 0.16, 0.22] → [0.14, 0.18, 0.24]; `stars.threshold` 0.94 → 0.935 and
`stars.size` 0.11 → 0.115, so a darker sky carries slightly more, slightly larger
stars. Day, tints, motion, look, form, snow, weather untouched. All inside the
constitution's ranges (a/b 0–1, threshold 0.90–0.98, size 0.05–0.20).

Lowering the offset and raising the amplitude together is deliberate: the night gets
darker *and* gains contrast, rather than simply dimming. Generation 4 had prepared a
half-size version of this step ([0.10, 0.12, 0.17] / [0.13, 0.17, 0.23]). Measured, it
moved mean luminance by 0.003 — below anything a room would notice, a generation spent
on a rounding error. So the step was doubled after walking the ladder on the starry
night at a fixed frame:

| night a / b | mean luminance | luminance SD | mean rgb | near-black |
|---|---|---|---|---|
| [.11,.13,.18] / [.12,.16,.22] (gen 2, current) | 0.1600 | 0.0460 | 29, 42, 66 | 0.005% |
| [.10,.12,.17] / [.13,.17,.23] (gen 4's deferred step) | 0.1573 | 0.0473 | 28, 41, 66 | 0.023% |
| **[.09,.11,.16] / [.14,.18,.24] (chosen)** | **0.1546** | **0.0487** | **27, 40, 65** | **0.065%** |
| [.08,.10,.15] / [.15,.19,.25] | 0.1520 | 0.0501 | 26, 40, 65 | 0.129% |

The ladder is monotonic in the direction both standing verdicts ask for. Luminance SD
*rises* as the night deepens — more of the composition carried in luminance, which is
what verdict 1 demanded after the ochre soup. And red falls (29 → 27) while blue holds
(66 → 65), so the blue anchor verdict 2 asked to keep gets *more* dominant, not less.
The chosen step lands `a`'s red and green back at generation 0's pre-white-nights
values while `b` stays well above anything the piece has had, so the nights are as dark
as they were in spring but hold more structure than they ever did. Stopping at 2×
rather than 3× keeps this a drift.

It also nibbles at the flaw generation 5 flagged in itself — the khaki cast the new
self-shadowing pulls out of the night palette's warm side. Side by side at the same
state, generation 5 washes pale tan across most of the right half of the frame;
generation 6 confines it to darker, narrower veins under a deeper blue mass. Improved,
not solved: the dust is still visible and still the first thing to fix if a future run
wants the nights cleaner. `form.shade` is the dial, and it still has no sanctioned
range (see generation 5).

Gate, five states at a fixed frame, 1280×720, mood forced to target (gen 5 → gen 6,
mean luminance): clear day 0.1822 → 0.1822 **bit-identical**, as a night-only change
must be — a useful check that the harness was measuring what it claimed; overcast
night 0.1706 → 0.1668 (34,44,63 → 33,43,62); starry night 0.1600 → 0.1546; snowfall
0.2525 → 0.2521; storm 0.1475 → 0.1473; dusk at night=0.5 0.1626 → 0.1608, still plum.
All five inside 0.04–0.85 and clearly distinct in both luminance and hue. No near-white
above 0.03%, no near-black frame — the worst case is 0.065% of *pixels* below 0.02 in a
frame averaging 0.155. `gl.getError()` 0, no console errors. `DEFAULT_GENOME` verified
equal to genome.json; served alone with genome.json returning 404, clear day renders
the approved red-over-slate at generation 6, and the on-load and settled frames are
identical (0.1822, rgb 82/31/38-class), so cold starts and offline show this organism.

Identity clause, honestly: this is the mildest kind of mutation — no motion, no breath,
no shader code, one palette block and two star numbers. Still abstract, calm,
continuous; a palette cannot flash. The risk in a darker night is dullness rather than
drama, and the rising luminance SD is the evidence against that. It should read as
"the nights have gotten darker, the way they do in August."

**Method note, for future runs.** Two traps cost most of this run, both of which make a
validation *look* clean while measuring nothing. (1) The port in EVOLVE.md's example was
already held by another checkout's server; `http-server` failed to bind, and the browser
happily measured a stale tree that also said "generation 5". Always assert the served
generation *and the actual mutated values* from inside the page before trusting a number.
(2) The browser pane collapses to a 2×2 viewport when it is not fronted, so anything that
calls the page's `resize()` measures a 4-pixel image and reports plausible-looking
statistics. Set `canvas.width/height` explicitly instead. The reliable harness is to call
the page's own `frame(now)` synchronously with a pinned canvas, `mood` forced to `target`
and `G` forced to `Gt`, then `gl.readPixels` — deterministic, identical frame before and
after, and independent of pane visibility.

`proposals/002-green-as-a-third-base.md` was considered and deliberately left for a
later run: it is marked in free scope, but it warns in its own Season risk that green in
late August is against the year and suggests the night palette as its first home. This
generation takes the nights the other way, into darkness, so green waits — and if it is
ever tried on the night palette, it should be tried against *this* night, not gen 2's.
`proposals/001-cumulus-not-fog.md` was already closed by generation 5 and needed nothing
here. No proposal was written this run: the mutation was in free scope.
Verdict:

## generation 7 — the year starts to slow down (2026-08-14)

The first mutation to the **breath**. Six generations have gone by and nobody has
touched it: four were palette (1, 2, 3, 6), one was a repair (4), one was form (5).
The genome's oscillators have carried generation 0's numbers the whole way. A piece
that only ever changes colour is drifting along one dimension, and the constitution
names breath as one of the three timescales the thing is alive at — so this run takes
the seconds.

`breath` 7.3s/0.26, 11.9s/0.15, 28.6s/0.09 → **8.6s/0.22, 13.7s/0.15, 33.1s/0.13**.
Phases untouched. Every period ~16% longer, and amplitude moved off the fast oscillator
onto the slow one. Sum of amps stays at exactly 0.50 (cap 0.5), count stays at 3
(range 2–4), periods stay inside 4–60s. Nothing else in the genome changed — no
palette, no motion, no look, no form, no stars, no snow, no weather.

Mid-August in Stockholm: the light is going, the year is slowing. So does the organism.
The direction is also the one the identity clause asks for by default — *when in doubt:
quieter*.

**What breath actually drives, and why this mutation cannot flash.** Breath feeds one
uniform: `u_wmul = warpBase + warpBreath * breath`, i.e. turbulence between 0.85 and
1.03. It never touches colour, contrast, or brightness. Measured on the same frame,
generation 6's breath against generation 7's, across 15 fixed moments: mean luminance
differed by at most **0.0007** and averaged 0.1641 vs 0.1642, luminance SD 0.0769 vs
0.0768. Full-field luminance is, to within a rounding error, *identical*. Against the
hard physiological limit this is the safest class of mutation the genome permits — and
the fastest oscillator went from 7.3s to 8.6s, moving further from the 4s floor, not
toward it.

**Which raises the fair question: is it visible at all?** Yes, and it is visible as
tempo, which is the only place a breath change should show. Three measurements, all on
pinned 1280×720 frames with mood forced to target and `G` forced to `Gt`:

| | gen 6 breath | gen 7 breath | |
|---|---|---|---|
| mean half-cycle of the breath signal (4000s sim) | 3.65s | **4.30s** | +17.8% slower |
| peak d(breath)/dt | 0.3227/s | **0.2542/s** | −21.2% |
| image change per second (RMS luminance, 10 samples) | 0.00909 | **0.00809** | −11% |
| peak change rate | 0.01638 | **0.01262** | −23% |
| peak-to-trough of the change rate | 3.58× | **2.67×** | more even |

And the two breaths put the organism in genuinely different configurations: the
same-instant RMS luminance difference between them averages **0.01155** over eight
moments — *larger than a full second of the piece's own motion* (0.0081–0.0091/s). This
is not a rounding error dressed up as a generation.

**The honest critique, and it is a real one.** The peak-to-trough figure is the cost.
Generation 6 breathed unevenly — gusts and lulls, 3.58× between its fastest and slowest
moments. Generation 7 breathes at 2.67×: slower, deeper, and *more regular*. The long
swell got stronger, the seconds-scale gustiness got gentler. That is calmer, which the
constitution endorses, but calmer and more even is one step from inert, and the standing
verdict on generation 4 was that the piece was not good enough to keep on the wall. If
the human's read is that it lost life rather than gained calm, the fix is cheap and
known: the two dials are separable — put amplitude back on the 8.6s oscillator without
shortening the periods, and the tempo stays slow while the gustiness returns.

Which names this run's one methodological confound plainly: **period and amplitude
distribution were moved together.** It is one coherent change to one genome block, so
it is one mutation, but it is not one *variable*. A future run wanting to know which
half did the work should move only one.

Gate, five states, three moments each (ts = 12/34/57s) so a breath-modulated state is
judged across its cycle rather than at one phase. 1280×720, mood forced to target,
`gl.getError()` 0 in every sample:

| state | mean luminance | luminance SD | mean rgb (worst sample) | near-black | near-white |
|---|---|---|---|---|---|
| clear day | 0.1532 – 0.2086 | 0.071 – 0.110 | 57,30,37 → 87,38,42 | 0% | 0% |
| overcast night | 0.1621 – 0.1709 | 0.043 – 0.047 | 31,42,62 | 0.030% | 0% |
| starry night | 0.1471 – 0.1748 | 0.040 – 0.048 | 25,39,64 | 0.169% | 0.001% |
| snowfall | 0.2453 – 0.2603 | 0.075 – 0.083 | 73,61,76 | 0% | 0.017% |
| storm | 0.1538 – 0.1649 | 0.036 – 0.048 | 39,38,47 | 0% | 0% |

All inside 0.04–0.85, no near-black or near-white frame anywhere (the worst case is
0.169% of *pixels* below 0.02 in a frame averaging 0.147), and the five are clearly
distinct in both hue and structure: red-over-slate day, blue overcast night, blue with
stars, pale hush over faint red for snow, flat slate with muted red beneath for storm.
Shader compiles; the only console error in the whole run was the deliberate 404 in the
offline test below.

Both fallback gates pass. `DEFAULT_GENOME` verified equal to genome.json by the EVOLVE.md
one-liner ("fallback in sync, generation 7"). Served alone with genome.json returning
404, the page reports generation 7 with the mutated breath and the approved red day
palette, and the on-load frame and the settled frame are **bit-identical** (0.1771,
rgb 71/33/38) — and equal to the same moment on the normally-served page, so cold starts
and offline show this organism and not an ancestor.

Identity clause: abstract, calm, continuous, unmistakably the same ink. No shader code
was touched. It should read as *"it's breathing more slowly than it was."*

**Still open, third generation of asking.** The khaki cast the generation-5 shading
pulls out of the night palette's warm side is unchanged — visible in this run's overcast
and starry screenshots as tan veins around a blue mass. `form.shade` is the dial and it
*still* has no sanctioned range in CONSTITUTION.md, which the agent may not edit, so no
run can legally touch it. Suggested line under free scope's genome ranges, unchanged
from generation 5: `form: shade 0–0.6`.

`proposals/002-green-as-a-third-base.md` waits another run, for the reason it gives
itself: green in late August is against the year. Nothing has changed that. No proposal
was written this run; the mutation was in free scope.
Verdict:
