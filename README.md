# Pocket Front — PF-58 demo build

Static web build of **Pocket Front**, cut by the Build agent for the PF-58 human
checkpoint (PF-311).

* **Built from:** `tj4d/pocket-front` @ `7c97cad` (`main`, 2026-10-28)
* **Published as:** `tj4d/tj4d.github.io` @ `d47e89e`
* **Command:** `pnpm build:web` (Expo SDK 54 static export)
* **Contents:** the exported `apps/mobile/dist` only — no source, no secrets.
* **Play it:** open `https://tj4d.github.io/` in a browser.

This build replaces PF-297's (`8be7c88`) and carries the **two shipped fixes that were
still missing from it** — both of them things the player can see.

* **PF-306 — the museum's unit caption reads whole.** The caption sliced its line to a
  flat 32 characters, so at 100 % the flavour was silently shortened to
  `THE KIT IS PACKED PRECISELY; THE`, and at 130 % the line was 384 px wide in a 360-px
  frame and was cut at the edge (`THE KIT IS PACKED PRECISEL`). The caption now asks its
  face of the room — **heavy at 100 %, mono at 130 %** — and the flavour is body copy
  that **wraps on whole words**. This build reads
  `THE KIT IS PACKED PRECISELY; THE BISCUITS HAVE OTHER ARRANGEMENTS.` whole at both
  scales, **29 logical px clear of the caption's frame** at both.
* **PF-302 — Command Post Raid's aim and effect agree.** The card declared
  `targets: "enemyUnit"` but its effect resolved `commandPost`, so the 12 damage landed
  on the Command Post whatever the player picked, and the `+` version had no
  `costDelta` and dealt **11** — the same cost for one less damage. This build's card
  reads `Deal 12 to the chosen enemy.`, and its `+` version
  `Deal 12 to the chosen enemy. Costs 1 less Supply.` — the same 12 for 1 less Supply.

**Nothing else moved.** `git diff --stat 8be7c88 7c97cad` over `apps/`, `packages/`,
`art/` and `assets/` is **four files**: PF-306's three under `apps/mobile/src/render/meta/`
and PF-302's one card JSON under `packages/content/data/cards/`. The engine, the corpus,
the art and the simulator are unchanged, so the game rules and the measured figures are
unchanged from PF-297's build.

It also still carries everything PF-297's cut carried: **PF-294** (the Order of Battle
sheet reads its unit names whole — `FALLSCHIRMJÄGER`, `KUBELWAGEN RECON`), **PF-296**
(three readings whole at the 130 % system font scale), **PF-298** (the Order of Battle
sheet's header counter stacks at 130 %), **PF-293** (the map's legend rows read whole at
130 %), **PF-291** (five screens no longer cut their own labels) and **PF-284** (the
React hydration error `#419`/`#418` off the console).

The bundle is **3 467 982 B = 3.31 MB of JS in 13 chunks** (entry 2 182 598 B,
`__common` 991 348 B, HandCanvas 247 002 B) across **9 static routes**, 11 741 740 B
on disk in 44 files (45 with this README), with the 8 001 100 B CanvasKit wasm and the
23 292 B atlas.

**Verified on this artefact**, from outside the build machine, in headless Chromium at
**360×640 logical, DPR 3**, a fresh browser context per route:

* **all nine routes answer 200, with 0 console errors and 0 page errors** — no `#419`,
  no `#418`, and 0 warnings;
* **every reading above reads whole at 100 % and at 130 %** — the title `POCKET FRONT`,
  the selection `THE BOX IS OPEN`, the map's legend caption
  `NEXT MARCH · TAP A LIT NODE`, the map's legend row `SUPPLY · ROW 4`, the map's
  empty-state sentence `NO RUN IN PROGRESS · CHOOSE YOUR COMMAND FIRST`, the War
  Record's `CAMPAIGN LEDGER` and its desk footer `RUNS 0 COMPLETED / 0 STARTED`, the
  Order of Battle sheet's `FALLSCHIRMJÄGER` and `KUBELWAGEN RECON` and its header
  counter `6 OF 8 · 1 LOST`, and the museum's caption
  `THE KIT IS PACKED PRECISELY; THE BISCUITS HAVE OTHER ARRANGEMENTS.`;
* the museum's caption is measured, not eyeballed: its ink ends **29 logical px inside**
  the caption's frame at both 100 % and 130 %, so no character is lost at the frame's
  edge;
* the **negative control** — the previously published bytes (`88c4087`) driven by the
  same script — read `THE KIT IS PACKED PRECISELY; THE` at **both** scales, and at
  130 % that line was **53 logical px past the 360-px canvas**, so the script can see
  the truncation the old bytes carry and the fix the new bytes do not. The old bytes'
  Command Post Raid card read `Deal 12 to the enemy Command Post.`, with a `+` version
  of `Deal 11 to the enemy Command Post.` and no cost reduction.

**Cold starts**: 24 cold runs with an empty cache, in headless Chromium at 360×640 /
DPR 3 — **median first contentful paint 766 ms, median menu-interactive 754 ms**
(min 436 / 429.1 ms, max 2 972 / 2 945.7 ms), **4 012 244 B** over the wire at the
median, and **2 of 24 over the 2.5 s budget** — both in runs whose §7 pre-window
CPU-idle reading was under 22 %. The box was **contended** for most of that sample
(the pre-window idle ran from 0 % to 80.42 %, median 27.89 %), so these medians are an
**upper bound** on the artefact's cold-start cost, not a quiet-box measurement; the one
run taken on a §7-valid window (80.42 % idle) read **664 ms** first contentful paint and
**652.1 ms** menu-interactive. QA's re-measurement of the published artefact is the
number that should go on the run sheet.

**One reading is still cut**, and it was cut in the previous build too: the War Record's
`NO NEW UNLOCKS THIS LEVEL` row is 300 px wide at 130 % and its frame has room for
288 px, so its last character is lost. It is in the museum's **WAR** tab, it is not on
the demo's path, and it is unchanged by this cut.

The run sheet Trevor walks is `docs/DEMO.md` in the source repository. Known issues for
this build are listed on Multica issue PF-311.
