# Pocket Front — PF-58 demo build

Static web build of **Pocket Front**, cut by the Build agent for the PF-58 human
checkpoint (PF-297).

* **Built from:** `tj4d/pocket-front` @ `8be7c88` (`main`, 2026-10-27)
* **Command:** `pnpm build:web` (Expo SDK 54 static export)
* **Contents:** the exported `apps/mobile/dist` only — no source, no secrets.
* **Play it:** open `https://tj4d.github.io/` in a browser.

This build replaces PF-292's (`de2b16d`) and carries the **four client merges that were
still missing from it** — **PF-294, PF-296, PF-298 and PF-293** — all four in
`apps/mobile/**`.

**It also carries two changes that are not client-side, and they change the game's
numbers.** The previous published build was cut before them:

* **ADR-0015's lever (PF-299)** — the enemy's Command Post now resolves its hit points
  **per nation** (germany 56, usa 71, ussr 56) instead of a flat 46. This is in
  `packages/engine`.
* **PF-213** — the nine commander kits are re-drawn by deck and roster swaps, so no
  number moved but every kit's silhouette changed. This is in `packages/content`.

The **art and the simulator are unchanged**. So the measured figures moved: the
simulator on this tree reads a **44.0 % win rate** (was 81.0 %) and a **9.00-turn**
median battle (was 6.00) — the §11.4 band is inside 35–55 % on this build for the
first time.

* **PF-294 — the Order of Battle sheet reads its unit names whole.** The sheet's
  row cut the name in silence: it read `FALLSCHIRMJÄGE`. This build reads
  `FALLSCHIRMJÄGER` and `KUBELWAGEN RECON` whole.
* **PF-296 — three more readings are whole at the 130 % system font scale** — the
  menu's kicker, the select screen's `MARCH` button against the campaign row, and
  the War Record's desk footer.
* **PF-298 — the Order of Battle sheet's header counter stacks at 130 %**:
  `ORDER OF BATTLE` on line one, `6 OF 8 · 1 LOST` whole on line two.
* **PF-293 — the map's legend rows read whole at 130 %.** The rows drop to the mono
  face, because the heavy face cannot hold an eighteen-character label in the row's
  272-px room at 130 % (it wants 360 px) and the mono face can (216 px) —
  `SUPPLY · ROW 4` inside the row's own frame.

It also still carries everything PF-292's cut carried: **PF-291** — five screens no
longer cut their own labels, so the title screen reads `POCKET FRONT`, the selection
screen `THE BOX IS OPEN`, the map's legend caption `NEXT MARCH · TAP A LIT NODE`, the
map's empty state `NO RUN IN PROGRESS · CHOOSE YOUR COMMAND FIRST` and the War
Record's desk title `CAMPAIGN LEDGER` — and **PF-284**, which took the React
hydration error (`#419`/`#418`) off the console.

The bundle is **3 465 206 B = 3.31 MB of JS in 13 chunks** (entry 2 180 208 B,
`__common` 990 965 B, HandCanvas 247 003 B) across **9 static routes**, 11 734 483 B
on disk in 44 files (45 with this README), with the 8 001 100 B CanvasKit wasm and
the 23 292 B atlas.

**Verified on this artefact**, from outside the build machine, in headless Chromium at
**360×640 logical, DPR 3**, a fresh browser context per route:

* **all nine routes answer 200, with 0 console errors and 0 page errors** — no `#419`,
  no `#418`;
* **every reading above reads whole at 100 % and at 130 %** — the title `POCKET FRONT`,
  the selection `THE BOX IS OPEN`, the map's legend caption
  `NEXT MARCH · TAP A LIT NODE`, the map's empty-state sentence
  `NO RUN IN PROGRESS · CHOOSE YOUR COMMAND FIRST`, the War Record's `CAMPAIGN LEDGER`,
  the Order of Battle sheet's `FALLSCHIRMJÄGER` and `KUBELWAGEN RECON`, its header
  counter `6 OF 8 · 1 LOST`, and the map's legend row `SUPPLY · ROW 4`;
* the three **130 %** fixes are visible in the pixels: this build matches the committed
  130 % baselines at **0 %** on the menu, the selection screen and the War Record's WAR
  tab, where the previously published bytes differ by **38.17 %**, **34.57 %** and
  **4.01 %**;
* the **negative control** — the previously published bytes (`8312db9`) driven by the
  same script — reads `FALLSCHIRMJÄGE` instead of `FALLSCHIRMJÄGER`, and its
  `KUBELWAGEN RECON` row is cut as well.

One reading is **still cut**, and it was cut in the previous build too: the museum's
unit caption loses the last two characters of its line at 130 % (PF-306). It is not on
the demo's path.

The run sheet Trevor walks is `docs/DEMO.md` in the source repository. Known issues
for this build are listed on Multica issue PF-297.
