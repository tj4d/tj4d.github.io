# Pocket Front — PF-58 demo build

Static web build of **Pocket Front**, cut by the Build agent for the PF-58 human
checkpoint (PF-263).

* **Built from:** `tj4d/pocket-front` @ `7177524` (`main`, 2026-10-26)
* **Command:** `pnpm build:web` (Expo SDK 54 static export)
* **Contents:** the exported `apps/mobile/dist` only — no source, no secrets.
* **Play it:** open `https://tj4d.github.io/` in a browser.

This build is cut from the tree that ships, after **PF-260** and **PF-249** have
merged. **The corpus is different from the previous published build**, and in a way
the player sees:

* **PF-252** — the corpus's **40 authored reinforcements** were re-aimed. **21 of
  them used to be aimed at column 6**, the enemy's own back column; the player now
  meets a different corpus.
* **PF-260** — the corpus's **last three second waves** land in the row the enemy's
  own advance does not own, so every authored wave now fires.
* **PF-249** — PF-219's option A: an **8-turn cap on all 40 encounters**, **+4 attack
  on 36 units** and **+4 card damage on 19 cards**. This is the change that moves
  §11 criterion 5 from NOT MET to MET — a 6-turn median battle and a 45-minute
  median run, against §11.5's 3–6 turn and 35–55 minute bands.
* **PF-248** — the Supply Depot's always-on actions sit **below** the offers list and
  the offers window is capped at the **130 % font scale**, so `LEAVE DEPOT` can no
  longer be scrolled away or hidden behind a refusal. At 1× nothing moved.

It also carries the client and engine work merged since the last cut:
**PF-242** (the Well-Thumbed Field Manual's rule reaches the client), **PF-276**
(the reward pool and the Supply Depot offer only the unlocked Field Assets),
**PF-279** (a doctrine purchase grants the commander it sells), **PF-277** (the
unlock filter gates the ladder's table), **PF-271** (the battle HUD's metrics are
legible and separate at 130 %), **PF-274** (a run granted an asset at start holds
its id), **PF-256** (the kit's `PixelTextBlock` draws every row inside its own box),
**PF-273** (a negative `loseMorale` amount reads its sign), **PF-272** (a verdict
trigger that changes the board now changes it), **PF-269** (the app hands the engine
the assets' authored definitions), **PF-233** (the battle HUD's `MENU` button no
longer runs off the right edge of a 360-px screen), **PF-270** (an asset acquired
mid-run arrives with its definition), **PF-268** (the Well-Thumbed Field Manual's
passive fires), **PF-264** (the event route's panel headers fit their box at 130 %)
and **PF-261** (the event route's controls fit their labels at 130 %).

It replaces PF-257's build from `main @ 2caaa89`, which predated all of the above —
most visibly the corpus's re-aim and PF-249's rebalance.

The bundle is **3 452 310 B = 3.29 MB of JS in 13 chunks** (entry 2 165 388 B,
`__common` 990 965 B, HandCanvas 247 003 B) across **9 static routes**, 11 721 404 B
on disk with the 8.00 MB CanvasKit wasm and the 23 292 B atlas.

The run sheet Trevor walks is `docs/DEMO.md` in the source repository. Known issues
for this build are listed on Multica issue PF-263.
