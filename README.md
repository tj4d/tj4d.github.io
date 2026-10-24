# Pocket Front — PF-58 demo build

Static web build of **Pocket Front**, cut by the Build agent for the PF-58 human
checkpoint (PF-225).

* **Built from:** `tj4d/pocket-front` @ `49b7793` (`main`, 2026-10-24)
* **Command:** `pnpm build:web` (Expo SDK 54 static export)
* **Contents:** the exported `apps/mobile/dist` only — no source, no secrets.
* **Play it:** open `https://tj4d.github.io/` in a browser.

This build includes PF-216 (the nine sector-1/2/3 `SECTOR_COMMAND` boss encounters)
**and** PF-223 (the client's content manifest, which is what carries those nine into
the app), so a run now fights three real sector objectives instead of three empty
boards. It also carries PF-222 (the mono pixel font's `_` glyph, so the battle ticker
reads `SU_PENAL_COMPANY` and not `SU?PENAL?COMPANY`), PF-218, PF-224, PF-226 and
PF-229.

It replaces PF-210's build from `main @ fd7235c`, which predates PF-216 and therefore
still played three empty sector-command boards every run.

The run sheet Trevor walks is `docs/DEMO.md` in the source repository. Known issues
for this build are listed on Multica issue PF-225.
