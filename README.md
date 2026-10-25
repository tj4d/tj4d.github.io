# Pocket Front — PF-58 demo build

Static web build of **Pocket Front**, cut by the Build agent for the PF-58 human
checkpoint (PF-234).

* **Built from:** `tj4d/pocket-front` @ `a78f8e3` (`main`, 2026-10-25)
* **Command:** `pnpm build:web` (Expo SDK 54 static export)
* **Contents:** the exported `apps/mobile/dist` only — no source, no secrets.
* **Play it:** open `https://tj4d.github.io/` in a browser.

This build carries **PF-217**, the UI-kit fix that paints a button as a button:
the battle screen's `END TURN` is now a 144 × 40 filled, centred button instead of
a thin blue bar, the victory screen's `CONTINUE` is a filled button, the Supply
Depot's offer rows read `CLOSE ASSAULT — 35 RQ` and `AMMO CRATES — 35 RQ` instead of
`CLOSE F` and `AMMO CR`, and the victory panel's last line no longer sits on its
bottom border. It also carries **PF-216** (the nine sector-1/2/3 `SECTOR_COMMAND`
boss encounters) and **PF-223** (the client's content manifest, which is what
carries those nine into the app), so a run fights three real sector objectives
instead of three empty boards; plus PF-222 (the mono pixel font's `_` glyph, so the
battle ticker reads `SU_PENAL_COMPANY` and not `SU?PENAL?COMPANY`), PF-218,
PF-224, PF-226 and PF-229.

It replaces PF-225's build from `main @ 49b7793`, which predates PF-217 and
therefore still showed every button as a thin blue bar with its label in it.

The run sheet Trevor walks is `docs/DEMO.md` in the source repository. Known issues
for this build are listed on Multica issue PF-234.
