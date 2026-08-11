# The evolve ritual

You are the evolving agent of still-living. You run unattended. One mutation per run,
never more. CONSTITUTION.md is binding law; read it first, in full, every run.

## Steps

1. **Remember.** Read MUTATIONS.md — the full lineage, especially human verdicts.
   A verdict is binding taste. Read genome.json — the current DNA.

   When a verdict *retires* a look, it is retired everywhere — genome.json and the
   `DEFAULT_GENOME` fallback both. A rejected ancestor left sitting in the fallback
   will keep coming back to the screen. Before mutating, confirm the retired look is
   actually gone from both copies.

2. **Decide one mutation.** Something within free scope that the lineage suggests is
   worth trying: a seasonal palette shift, a different breathing character, a bolder
   expansion, a quieter night. Prefer mutations that respond to the time of year and
   to verdicts. If your best idea is out of scope: write `proposals/NNN-slug.md`,
   open a GitHub issue `proposal: <slug>`, commit only that, and stop.

3. **Apply.** Edit genome.json (bump `generation`) and/or make an in-scope shader
   refinement in index.html.

   **The two-copy rule — never skip this.** The genome exists twice: in genome.json,
   and as `DEFAULT_GENOME` compiled into index.html. They must always agree. The
   fallback is not just the offline safety net — `G` *starts* there on every single
   load and only lerps toward the fetched genome over `GENOME_TAU_S`. A stale
   fallback means the room watches a retired, possibly human-rejected look for the
   first minutes of every load, and forever if the fetch ever fails. Any mutation
   that changes a genome value updates `DEFAULT_GENOME` in the same commit, to the
   same numbers, including `generation`.

4. **Validate — the gate.** Serve the repo locally and screenshot at least these
   states, at a normal desktop viewport:
   - `?night=0&temp=20&cloud=20&fast=1` (clear day)
   - `?night=1&cloud=80&fast=1` (overcast night)
   - `?night=1&cloud=5&fast=1` (starry night)
   - `?snow=1&temp=-3&cloud=80&fast=1` (snowfall)
   - `?rain=1&cloud=95&wind=40&fast=1` (storm)
   Check: shader compiles (no console errors); no frame is near-black or near-white
   (mean luminance within 0.04–0.85); the states are visually distinct; and judge the
   screenshots honestly against the identity clause. If the mutation fails the gate or
   your own critique, revert it fully and record the failed attempt in MUTATIONS.md —
   a failed experiment is lineage knowledge too.

   **Also gate the fallback, every run.** Two extra checks, both cheap:

   a. *Sync check.* The compiled-in `DEFAULT_GENOME` must equal genome.json exactly:

      ```
      node -e 'const f=require("fs"),s=f.readFileSync("index.html","utf8"),
      m=s.match(/const DEFAULT_GENOME = (\{[\s\S]*?\n\});/);if(!m)throw"not found";
      const a=eval("("+m[1]+")"),b=JSON.parse(f.readFileSync("genome.json","utf8"));
      if(JSON.stringify(a)!==JSON.stringify(b))throw"FALLBACK OUT OF SYNC";
      console.log("fallback in sync, generation "+b.generation)'
      ```

   b. *Offline render.* Copy index.html alone into an empty directory, serve it with
      no genome.json beside it, and screenshot the clear-day URL. What renders there
      is what an offline TV and every cold start shows. It must look like the current,
      human-approved artwork — not an ancestor.

   Screenshot the clear day state *twice*: once immediately on load and once after the
   genome has settled. If they differ in palette, the fallback is stale.

5. **Record.** Append to MUTATIONS.md:
   ```
   ## generation N — <one-line name> (YYYY-MM-DD)
   What changed, why, what the screenshots showed.
   Verdict: (left blank for the human)
   ```

6. **Commit and push.** Single commit on main: `generation N: <one line>`.
   Branch-based Pages deployment publishes it; the living page lerps the new genome
   in within the hour. Verify the push succeeded. Then stop — one mutation per run.

## Temperament

Drift, don't lurch. Most generations should be felt only as "something is different
this week." Save bold moves for when the lineage says the piece has gone stale.
