# Pocket Front — PF-58 demo build

Static web build of **Pocket Front**, cut by the Build agent for the PF-58 human
checkpoint (PF-326).

* **Built from:** `tj4d/pocket-front` @ `5fa8331` (`main`, 2026-10-28) — full SHA
  `5fa8331350f6e289bc3523b18507df15290e933c`
* **Published as:** `tj4d/tj4d.github.io` @ `44045b6`
* **Command:** `pnpm build:web` (Expo SDK 54 static export)
* **Contents:** the exported `apps/mobile/dist` only — no source, no secrets.
* **Play it:** open `https://tj4d.github.io/` in a browser.

This build replaces PF-317's (`cf8ec39`) and carries the **three shipped fixes that were
still missing from it** — the three defects QA walked on the previously published bytes
and found on the demo path, and all three of them things the player can see.

* **PF-322 — the hand's cards are playable, and they are painted in the nation colour.**
  The hand asked the engine `canPlay` with **no aim**, so every card that takes an aim
  came back refused — *"Card de_commit_grenadiers. de_commit_grenadiers has to be aimed
  at one of 3 cells"* — and the hand **painted it grey** while the board highlighted no
  cell at all. The rules were right; the question was wrong. An aimed card is now judged
  **at a cell the rules will take** (`availableTargets(request)[0]`), so a card the rules
  will take carries its own name, is painted in its nation's colour, and the board
  highlights the cells it may go to.
* **PF-330 — a card dropped on a legal cell resolves.** The commit path asked the engine
  about the **lifted** board, where the card is out of the hand, so `canPlay` always
  refused it (*"card c9 is not in hand"*) and the commit branch was unreachable: every
  drop was a take-back and **no card was ever played by pointer**, on any run, on any
  cell. The drop now **lifts the card and then resolves it**, so a card played by
  pointer leaves the hand for good and the board shows its effect.
* **PF-323 — the run reaches the disk.** The store patched a setter that no mutation
  path went through (zustand v5 hands the initializer the **raw** setter, and the immer
  middleware's wrapper calls that one in its closure), so the published app wrote
  **nothing** to `localStorage` while a run was played and **a refresh lost the run**.
  The write now lives on the one path every shell mutation takes, so the run slot
  `window.localStorage['pocket-front.save\run.current.v1']` holds the new state after
  every dispatch, and a reload brings the run back.

**Nothing else moved.** `git diff --stat 9557a8b 5fa8331` over `apps/`, `packages/` and
`art/` is **eleven files**, every one of them under `apps/mobile/src/` — PF-322's and
PF-330's in `apps/mobile/src/render/cards/`, PF-323's in `apps/mobile/src/store/`. The
engine, the corpus, the art and the simulator are unchanged, so the game rules and the
measured figures are unchanged from PF-317's build.

It also still carries everything PF-317's cut carried: **PF-313** (the War Record's
unlocks row reads whole), **PF-315** (the pick screen offers one commander per nation,
ADR-0018), **PF-306** (the museum's caption reads whole), **PF-302** (Command Post
Raid's aim tells the truth), **PF-294** (the Order of Battle sheet reads its unit names
whole), **PF-296**, **PF-298**, **PF-293**, **PF-291** and **PF-284** (the React
hydration error `#419`/`#418` off the console).

The bundle is **3 471 246 B = 3.31 MB of JS in 13 chunks** (entry 2 186 034 B,
`__common` 990 965 B, HandCanvas 247 217 B) across **9 static routes**, 11 740 559 B on
disk in 43 files (44 with `.nojekyll`, 45 with this README), with the 8 001 100 B
CanvasKit wasm and the 23 292 B atlas. The live index serves
`entry-003b0bc2073dd82a55ca70ac320b46ee.js`,
`__common-6a919a4b32977f6ee6e369def96d71ee.js` and
`__expo-metro-runtime-0c48b0beee2d3ce6030b475fcc5b1846.js` — and **all nine route
shells reference the same three bundles**, so the nine routes are one build.

**Verified on this artefact**, from outside the build machine, in headless Chromium at
**360×640 logical, DPR 3**, a fresh browser context per route:

* **the published tree is the served tree — 45 of 45 files byte-identical** by SHA-256,
  **0 differing, 0 not served**, and every asset the served route shells reference is in
  that tree;
* **all nine routes answer 200, with 0 console errors and 0 page errors** — no `#419`,
  no `#418`. The menu route alone draws **four WebGL driver warnings**
  (`GPU stall due to ReadPixels`); they are the measuring box's software GL, they
  reproduce identically on the previously published bytes, and they are reported rather
  than rounded to zero;
* **the three fixes this cut exists for, measured on the served bytes** — the hand's
  cards and their painted colour, the dropped card's outcome, and the run slot's
  contents, each read off the live artefact and each read against the previously
  published bytes through the same script, so the comparison is a measurement and not
  an assertion.

**The three fixes, on these bytes, with the previously published bytes beside them:**

| | this artefact | the previously published bytes (`cf8ec39`) |
|---|---|---|
| **the hand's cards** | **5 of 5 painted in Germany's colour** (`rgb(63, 74, 63)`), **0 greyed**, each labelled `Card <id>.` | **5 of 5 greyed** (`rgb(107, 100, 128)`), each labelled `Card <id>. <id> has to be aimed at one of N cells` |
| **the legal cells while a card is dragged** | **1 cell highlighted** | **0 cells highlighted** |
| **a card dropped on a legal cell** | **3 of 3 `committed`** — the card leaves the hand for good and the board shows its effect | **0 committed, 3 `lifted`** — the artefact takes the card out of the hand and leaves it in its undo window, and the next drop answers `card c6 is not in hand` |
| **the run slot** `pocket-front.save\run.current.v1` | **8 407 B** after MARCH, **43 542 B** in the battle, **44 118 B** after END TURN — and a reload brings **all five cards** and the run's Morale and Supply back | **empty at every reading**, and a reload loses the run (`Morale 0`, `NO BTL, turn 0`) |

**Cold starts**, on the live URL, in headless Chromium at 360×640 logical / DPR 3, a
fresh browser context per run, with the `docs/AGENT_RULES.md §7` pre-window CPU-idle
reading taken from `/proc/stat` **before** the browser launches:

| | |
|---|---:|
| cold runs attempted / **valid** | 16 / **15** (1 sample lost to a navigation timeout on the live host) |
| the valid runs' §7 pre-window idle | **81.26 % – 96.69 %** — all above the 80 % floor |
| **median first contentful paint** | **412 ms** (min 336, max 3 112) |
| **median menu-interactive** | **402.9 ms** (min 325.2, max 3 102.3) |
| median first paint | 60 ms (min 40, max 964) |
| **runs over the 2.5 s budget** | **1 of 15** |
| **bytes over the wire** (median) | **4 011 657 B** (min 763 986, max 4 011 795) |

The one run over the budget is the same live-host stall that took the failed sample: it
pulled **763 986 B** over the wire against the other runs' **4 011 6xx B** — the page did
not finish fetching its bundle. It is reported here rather than dropped, and it is a
property of the host, not of the artefact.

The run sheet Trevor walks is `docs/DEMO.md` in the source repository. Known issues for
this build are listed on Multica issue PF-326.
