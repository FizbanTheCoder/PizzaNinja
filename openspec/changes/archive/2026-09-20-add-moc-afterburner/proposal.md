## Why

MOC currently has one sink — the super shuriken — and slam kills are the only way to charge it. Adding an afterburner gives the player a second, incremental spend option (flight for mobility) that competes with the burst spend (insta-kill), closing the MOC loop: slam-kill → MOC → fly to a better position → slam-kill again.

## What Changes

- Double-tap ↑ while airborne with `airJumps == 0` ignites an afterburner that propels the player upward at a constant rise speed.
- Holding ↑ keeps rising; releasing ↑ enters a hover (drains MOC continuously at 10/s).
- Pressing ↓ while afterburner is active transitions into a slam dive (existing slam mechanic — auto-disengages the afterburner).
- Landing on a platform auto-disengages the afterburner.
- While the afterburner is active the player is immune to enemy and hazard contact damage (the slam-dive at the end is the weaponised moment).
- A full MOC bar (100) buys 10 seconds of total flight (climb or hover).
- New orange flame visual under the player while the afterburner is active.
- New Polish menu hint: `Podwójne ↑ w powietrzu: dopalacz! (1 s = 1/10 MOC)`

## Capabilities

### New Capabilities

- `moc-afterburner`: MOC-powered afterburner — activation, flight physics, hover, slam integration, contact-immunity, cost model, and visual.

### Modified Capabilities

(None — power-bar charging, decay, and HUD requirements are unchanged.)

## Impact

- `pizzaninja.html` — single-file game: player state vars, update() physics/input, draw() render, menu text.
- No new files or external dependencies.
