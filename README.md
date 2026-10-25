# Pocket Front — PF-58 demo build

Static web build of **Pocket Front**, cut by the Build agent for the PF-58 human
checkpoint (PF-257).

* **Built from:** `tj4d/pocket-front` @ `2caaa89` (`main`, 2026-10-25)
* **Command:** `pnpm build:web` (Expo SDK 54 static export)
* **Contents:** the exported `apps/mobile/dist` only — no source, no secrets.
* **Play it:** open `https://tj4d.github.io/` in a browser.

This build carries **ADR-0014's implementation (PF-253)** — movement is measured in
columns: every unit advances exactly one column on the enemy's turn, a card moves its
target exactly as far as it says, and `moveCost` is a unit's weight rather than a
barrier. It also carries **PF-245** (the campaign map shows the engine's refusal on a
refused march), **PF-247** (a reinforcement whose authored cell is spoken for lands in
the first free cell of its own sector), **PF-246** (the app's bundle now carries the
**41 Intel Drop events**, so an Intel Drop node opens a real event screen with a title,
a body and choices — it no longer opens empty), **PF-243** (the event route shows the
engine's refusals), **PF-239** and **PF-238** (the Supply Depot and the reward offer no
longer offer an asset the run already holds), **PF-237** (the `CHOOSE_REWARD` reducer
writes the run it was given, and §3.5's dead-weight Deployment card **stays in your
deck** instead of leaving it), **PF-220** (the battle route's exits no longer strand the
run), **PF-241** and **PF-251**.

It replaces PF-234's build from `main @ a78f8e3`, which predated all of the above —
most visibly the Intel Drop, the dead-weight card rule and ADR-0014 itself.

The run sheet Trevor walks is `docs/DEMO.md` in the source repository. Known issues
for this build are listed on Multica issue PF-257.
