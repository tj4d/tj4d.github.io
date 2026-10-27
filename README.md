# Pocket Front — PF-58 demo build

Static web build of **Pocket Front**, cut by the Build agent for the PF-58 human
checkpoint (PF-285).

* **Built from:** `tj4d/pocket-front` @ `41e4391` (`main`, 2026-10-26)
* **Command:** `pnpm build:web` (Expo SDK 54 static export)
* **Contents:** the exported `apps/mobile/dist` only — no source, no secrets.
* **Play it:** open `https://tj4d.github.io/` in a browser.

This build replaces PF-263's (`7177524`) and carries the **three client-side fixes**
that build was missing. **All three are in `apps/mobile/**` — the engine, the corpus,
the art and the simulator are unchanged from the previous published build.**

* **PF-284** — **the console is clean.** The static export's server-rendered shell and
  the client's first paint now agree, so the React hydration error (**minified #419**)
  that the previous build logged on `/`, `/run/battle` and `/run/map` is gone. The
  screen draws only after the client has mounted, which leaves native untouched.
  The export's seven route shells are byte-identical to one another.
* **PF-281** — **a Doctrine Tree node that sells a commander now grants it.** Buying
  *Combined Arms Doctrine* really puts *The Old Guard* in the collection, and the grant
  survives a restart. It is bought through the engine's own grant rule and written back
  through the profile write-back path.
* **PF-278** — **the battle HUD's turn block reads whole at the 130 % system font
  scale.** The block grows to hold the widest owner word (`NO BTL`) whole, so `ENEMY`
  is no longer a clipped fragment.

Also merged since PF-263's cut, and carried: **PF-286** (a measurement-only change to
the simulator's CLI — the shipped Command Post still has **46 HP**), plus the
`docs/**` and `CLAUDE.md` work of **PF-275**, **PF-265**, **PF-283**, **PF-267** and
**PF-284**.

It replaces PF-263's build from `main @ 7177524` — the build that was missing all
three of the fixes above.

The bundle is **3 454 815 B = 3.29 MB of JS in 13 chunks** (entry 2 167 506 B,
`__common` 991 348 B, HandCanvas 247 002 B) across **9 static routes**, 11 723 840 B
on disk with the 8 001 100 B CanvasKit wasm and the 23 292 B atlas.

The run sheet Trevor walks is `docs/DEMO.md` in the source repository. Known issues
for this build are listed on Multica issue PF-285.
