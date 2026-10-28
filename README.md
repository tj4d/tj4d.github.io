# Pocket Front — PF-58 demo build

Static web build of **Pocket Front**, cut by the Build agent for the PF-58 human
checkpoint (PF-317).

* **Built from:** `tj4d/pocket-front` @ `9557a8b` (`main`, 2026-10-28)
* **Published as:** `tj4d/tj4d.github.io` @ `cf8ec39`
* **Command:** `pnpm build:web` (Expo SDK 54 static export)
* **Contents:** the exported `apps/mobile/dist` only — no source, no secrets.
* **Play it:** open `https://tj4d.github.io/` in a browser.

This build replaces PF-311's (`d47e89e`) and carries the **two shipped fixes that were
still missing from it** — both of them things the player can see.

* **PF-313 — the War Record's unlocks row reads whole.** The row read its reading
  through a flat 26-character budget that knew nothing about the font scale. The
  reading is 25 characters and the mono cell is 6 logical px at 100 % and 12 at 130 %,
  so the row was 150 px at 100 % and **300 px at 130 %** — past the sheet's own paper
  edge, which took the last character in silence, and a second cut deeper: the sheet
  was cut at a fixed 156 px, so at 130 % the row sat below the paper's inner edge and
  **was not painted at all**. The row is now a `PixelTextBlock` given the sheet's own
  capacity, so it **wraps on whole words** (`NO NEW UNLOCKS THIS` / `LEVEL`) and reads
  whole at both scales.
* **PF-315 — the pick screen offers one commander per nation at launch.** The choice
  stack listed all three commanders of the chosen nation from the first minute, where
  **ADR-0018** says one per nation at launch — the Doctrine Tree's "unlock a
  commander" node and the War Record's ladder were two promises the screen did not
  keep. The screen now offers a commander only if the profile owns it: the launch
  commander from the first minute, the Doctrine Tree's once its node is bought, the
  ladder's once the War Record has climbed to its level.

**Nothing else moved.** `git diff --stat 7c97cad 9557a8b` over `apps/`, `packages/`
and `art/` is **ten files** — PF-313's four under `apps/mobile/src/render/meta/` and
PF-315's six under `apps/mobile/app/run/` and `apps/mobile/src/content/`. The engine,
the corpus, the art and the simulator are unchanged, so the game rules and the measured
figures are unchanged from PF-311's build.

It also still carries everything PF-311's cut carried: **PF-306** (the museum's caption
reads whole), **PF-302** (Command Post Raid's aim tells the truth), **PF-294** (the
Order of Battle sheet reads its unit names whole — `FALLSCHIRMJÄGER`,
`KUBELWAGEN RECON`), **PF-296** (three readings whole at the 130 % system font scale),
**PF-298** (the Order of Battle sheet's header counter stacks at 130 %), **PF-293** (the
map's legend rows read whole at 130 %), **PF-291** (five screens no longer cut their own
labels) and **PF-284** (the React hydration error `#419`/`#418` off the console).

The bundle is **3 469 837 B = 3.31 MB of JS in 13 chunks** (entry 2 184 453 B,
`__common` 991 348 B, HandCanvas 247 002 B) across **9 static routes**, 11 739 150 B
on disk in 43 files (44 with `.nojekyll`, 45 with this README), with the 8 001 100 B
CanvasKit wasm and the 23 292 B atlas.

**Verified on this artefact**, from outside the build machine, in headless Chromium at
**360×640 logical, DPR 3**, a fresh browser context per route:

* **all nine routes answer 200, with 0 console errors and 0 page errors** — no `#419`,
  no `#418`, and 0 warnings on the eight quiet routes (the menu route draws four **WebGL
  driver warnings** — `GPU stall due to ReadPixels` — which reproduce identically on the
  older bytes and so belong to the measuring box's software GL, not to this artefact);
* **every reading above reads whole at 100 % and at 130 %** — the title `POCKET FRONT`,
  the selection `THE BOX IS OPEN`, the map's legend caption
  `NEXT MARCH · TAP A LIT NODE`, the map's legend row `SUPPLY · ROW 4`, the map's
  empty-state sentence `NO RUN IN PROGRESS · CHOOSE YOUR COMMAND FIRST`, the War
  Record's `CAMPAIGN LEDGER` and its desk footer `RUNS 0 COMPLETED / 0 STARTED`, the
  Order of Battle sheet's `FALLSCHIRMJÄGER` and `KUBELWAGEN RECON` and its header
  counter `6 OF 8 · 1 LOST`, the menu's kicker `A TINY FRONT · A VERY LONG WAR`, and the
  museum's caption
  `THE KIT IS PACKED PRECISELY; THE BISCUITS HAVE OTHER ARRANGEMENTS.`;
* the museum's caption is measured, not eyeballed: its ink ends **inside** the caption's
  frame at both 100 % and 130 %, so no character is lost at the frame's edge;
* **the War Record's unlocks row is measured, not eyeballed**: at 130 % its ink is
  **226 px at logical x 40..266**, inside the sheet's own content column (x 32..328),
  and it reads `NO NEW UNLOCKS THIS` / `LEVEL` on two whole-word rows;
* **the pick screen offers exactly one commander per nation** — `The Staff Officer`
  (germany), `The Engineer` (ussr), `The Logistician` (usa) — the three launch
  commanders, and neither the ladder's nor the Doctrine Tree's;
* the **negative control** — the previously published bytes (`d47e89e`) driven by the
  **same script** — read **three commanders per nation** (`The Staff Officer`,
  `The Old Guard`, `The Panzer Ace` for germany; `The Commissar`, `The Engineer`,
  `The Line Holder` for ussr; `The Air Liaison`, `The Logistician`,
  `The Breakthrough Group` for usa), and its War Record row measured **298 px of ink at
  logical x 40..338 at 130 %** — **10 px past the content column's right edge at x 328**
  — and **not painted at all** in the screenshot. The script that reads one commander
  per nation and a contained row on this build reads three commanders and an overflowing
  row on that one, so it can see the difference the two builds carry.

**Cold starts**: 12 cold runs with an empty cache, in headless Chromium at 360×640 /
DPR 3 — **median first contentful paint 778 ms, median menu-interactive 761.6 ms** over
all twelve runs (min 400 / 391.3 ms, max 1 520 / 1 505.1 ms), **4 012 556 B** over the
wire at the median, and **0 of 12 over the 2.5 s budget**. The box was **contended**
for most of that sample: only **2 of the 12** runs cleared `docs/AGENT_RULES.md §7`'s
80 % pre-window CPU-idle floor (at 80.56 % and 85.85 %), because this host's own
`facefind.crawl_daemon` holds ~48 % of one core and caps the box's idle at about 65 %
(the same ceiling PF-213, PF-290, PF-299 and PF-308 recorded). These medians are an
**upper bound** on the artefact's cold-start cost, not a quiet-box measurement — QA's
re-measurement of the published artefact is the number that should go on the run sheet.

The run sheet Trevor walks is `docs/DEMO.md` in the source repository. Known issues for
this build are listed on Multica issue PF-317.
