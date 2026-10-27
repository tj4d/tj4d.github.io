# Pocket Front — PF-58 demo build

Static web build of **Pocket Front**, cut by the Build agent for the PF-58 human
checkpoint (PF-292).

* **Built from:** `tj4d/pocket-front` @ `de2b16d` (`main`, 2026-10-27)
* **Command:** `pnpm build:web` (Expo SDK 54 static export)
* **Contents:** the exported `apps/mobile/dist` only — no source, no secrets.
* **Play it:** open `https://tj4d.github.io/` in a browser.

This build replaces PF-285's (`41e4391`) and carries **PF-291** — the fix that stops
five shipped screens from cutting their own labels. **All of PF-291 is in
`apps/mobile/**` — the engine, the corpus, the art and the simulator are unchanged
from the previous published build.**

* **PF-291 — five screens no longer cut their own labels.** The kit's `PixelText`
  truncates with a hard character slice and no warning, so a reading longer than its
  character budget lost its tail in silence. The previous published build read
  `POCKET FRON` on the title screen and `THE BOX IS OPE` on the selection screen.
  This build reads:
  * `POCKET FRONT` — the title screen,
  * `THE BOX IS OPEN` — the selection screen,
  * `NEXT MARCH · TAP A LIT NODE` — the map's legend caption,
  * `NO RUN IN PROGRESS · CHOOSE YOUR COMMAND FIRST` — the map's empty state,
  * `CAMPAIGN LEDGER` — the War Record's desk title.

  Verified on this artifact at **360×640 logical, DPR 3**, in headless Chromium, at
  **100 % and 130 %** system font scale. At 130 % the map's two longer readings wrap
  onto a second row inside their panels rather than losing their tail, and the War
  Record's title is drawn in the game's mono face — the same scale, the other face
  (`docs/ART_BIBLE.md` §9) — because the heavy face cannot hold it there.

It also still carries everything PF-285's cut carried: **PF-284** (the console is
clean — the React hydration error is gone), **PF-281** (a Doctrine Tree node that
sells a commander really grants it) and **PF-278** (the battle HUD's turn block reads
whole at the 130 % system font scale).

The bundle is **3 455 283 B = 3.29 MB of JS in 13 chunks** (entry 2 167 974 B,
`__common` 991 348 B, HandCanvas 247 002 B) across **9 static routes**, 11 724 308 B
on disk in 44 files (45 with this README), with the 8 001 100 B CanvasKit wasm and
the 23 292 B atlas.

The run sheet Trevor walks is `docs/DEMO.md` in the source repository. Known issues
for this build are listed on Multica issue PF-292.
