# still-living

A living artwork in a single HTML file. No backend, no image model, no build step.
A still life that refuses to hold still.

**Live:** https://swepalm.github.io/still-living/

It is a real-time WebGL flow field — ink drifting through warped currents at 60fps —
whose mood is steered by the world outside:

- **wind** → tempo and turbulence
- **temperature** → warm/cool tint
- **cloud cover** → contrast and softness
- **rain** → grey-blue wash and a falling shimmer
- **snow** → a hushed pale wash with drifting flakes in three parallax layers
- **sun** → day palette (ochre / slate blue) fades to night palette (ink / cold teal),
  ramping over ~80 minutes around the real local sunrise and sunset
- **clear dark nights** → stars appear, twinkling through the thin parts of the ink

The field expands perpetually toward the viewer (two zoom layers crossfading, so it
never visibly resets), and its breathing is a blend of three incommensurate periods —
calm, but never repeating.

Weather comes from [Open-Meteo](https://open-meteo.com) (free, no API key) and refreshes
every 15 minutes. Mood changes drift in over ~45 seconds — the organism never jumps.
If the network is down it keeps living on its last mood.

## Run

Open `index.html` in any browser, or serve the folder:

```bash
npx http-server . -p 5180
```

Keys: `f` fullscreen · `h` show current mood.

## Cast to a TV

- **Chromecast / Google TV**: open the page in Chrome → ⋮ → Cast → cast the tab.
- **AirPlay**: open in Safari, AirPlay the window/screen.
- **TV browser / Fire TV**: open https://swepalm.github.io/still-living/ directly.

## Location

Defaults to Stockholm. Override via URL: `?lat=57.7&lon=11.97`

## Preview moods (debug)

Force a mood with URL params (skips live weather):

```
?night=1&temp=-5&cloud=90&wind=40&rain=1&fast=1
?snow=1&temp=-3&cloud=80&fast=1          # snowfall
?night=1&cloud=5&fast=1                  # clear starry night
```

## Adaptive quality

The canvas renders at a reduced internal resolution and adapts every 2s to hold
~60fps, so it stays smooth even on weak TV-stick browsers.
