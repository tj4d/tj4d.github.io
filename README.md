# Pocket Front — PF-58 demo build

Static web build of **Pocket Front**, cut by the Build agent for the PF-58 human
checkpoint (PF-210).

* **Built from:** `tj4d/pocket-front` @ `fd7235c` (`main`, 2026-10-24)
* **Command:** `pnpm build:web` (Expo SDK 54 static export)
* **Contents:** the exported `apps/mobile/dist` only — no source, no secrets.
* **Play it:** open `https://tj4d.github.io/` in a browser.

This build includes PF-205 (the campaign map draws one sector at a time) and
PF-211 (the battle screen has an end-turn control, the enemy turn resolves, and
there is a victory screen with the 1-of-3 reward choice).

The run sheet Trevor walks is `docs/DEMO.md` in the source repository. Known
issues for this build are listed on Multica issue PF-210.
