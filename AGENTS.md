# AGENTS.md

## What this is
A single-file HTML5 platformer (`pizzaninja.html`). Canvas is fixed 960x540. All game logic, rendering, and level data live in one inline `<script>`; there is no build system, package manager, tests, or lint. To verify changes, just open `pizzaninja.html` in a browser (no server needed). All user-facing text is Polish.

## Conventions
- Code style is deliberately condensed (single-letter vars like `p`, `K`, `ctx`, `cv`). Match it; don't "improve" readability wholesale.
- Add any UI text in Polish, matching the existing tone.
- Keep everything inside the one file; do not split into modules or add a build step without being asked.

## Workflow
- All OpenSpec artifacts and documentation entries are written in English.
- Every change gets a 2–3 sentence entry in `docs/changes.md` and in the "Change log" section of this file.
- After plan/artifacts are written, implementation starts only with the user's explicit consent.

## Level data (`LEVELS`)
Levels are plain object literals with tuple arrays. Exact formats:

```
{time: seconds, width: world width, sky: [topColor, bottomColor],
 spawn: [x, y], goal: [x],                       // goal is a 1-element array!
 platforms: [[x, y, w, h], ...],                 // world coordinates
 hazards:  [[x, y, w, h, vx, min, max], ...],    // patrols between min/max (world-x)
 robots:   [[x, y, w, h, vx, min, max, hp], ...]} // hp tier must be 1..3
```

- `goal[0]` is the x-coordinate; the goal box is drawn at fixed `y=380, w=80, h=100`.
- Robot color tier is indexed by `hp - 1` into the 3-element `TIER` array, so `hp` outside 1..3 breaks rendering/score.
- Robots score on death: `hpMax * 50`. Hazards are always `hp: 2`, score `+30`.
- `width` must exceed `960`; the camera clamps to `width - 960`.
- Ground platforms use `h: 60`, floating ones `h: 18` (rendering branches on `h > 20`).
- `sky` gradients darken by level index (hardcoded tiers `levelIdx < 2`, `< 4`), and `nf` star opacity in `drawSky` follows the same tiers. If you add levels, extend those tier checks.

## Mechanics
- Controls: A/D or arrow keys to move, Space = big jump, W/↑ = small jump, double-jump midair, Q/X = shuriken (`J` is unbound), ↓/S = slam (see below), R = restart level, Enter = advance through menu/intro/win/lose/done.
- Slam: armed only by a midair **Space** jump (`p.slamOk`); ↓/S while airborne and `invuln<=0` dives straight down (`p.slam`, `vy=1000`). Contact during slam insta-kills any robot tier or hazard (no damage to player, no bounce — chains possible); landing ends it. Kills route through `killRobot(j,mul)`/`killHazard(j,mul)` shared with shurikens.
- MOC (`power`, 0–100): charged by kills through a multiplier `mul` — slam `1` (`+hpMax*10`), normal shuriken `0.5` (`+hpMax*5`), super shuriken `0`. Only the killing hit pays; non-kill hits and platform-absorbed shurikens charge nothing, and firing costs nothing. Halved (`floor(power/2)`) on pit-fall, retry, restart, and level advance; 0 only on a fresh run. Third HUD line: cyan→pink bar, pulses silver at 50 and gold at 100. Spent on the super shuriken (Q/X, 50 per shot — 100 = two) and the afterburner (12.5/s, 8s per full bar).
- States: `menu`, `intro`, `play`, `win`, `lose`, `done` (`retry` is transient in `startLevel`).
- `dt` is clamped to `0.033` in `loop`; antgameloop uses `performance.now()` +
  `requestAnimationFrame`. Don't break the clamp or `last` tracking.
- Player hit by hazard/robot: knockback, 1s invulnerability, -3s; falling below `H+80`: -5s respawn.

## Change log
- **2026-09-28 — widen-level-seven-valleys**: The six lowest platforms of level 7 (four at `y=440`, two at `y=420`) are now 72 px wide instead of 48 or 70, which widens the landing window on the level's five worst descent landings — the full-speed overshoot drops from +72/+76/+80 px to +48/+52/+56 px, and the opening jump `P0→P1` becomes landable at full speed with zero precision required. Landing tolerance is exactly the platform width, so width was the only lever available, and every pad was grown to the right so the extra room lands on the edge the player arrives at. The two 70 px "rest beats" are normalised to 72 px, retiring that exception in favour of a rule the level can be read by — valley floors are 72 px, peaks are 48 px; mechanics, timers, hazard, robot, and every other level are unchanged.
- **2026-09-28 — cheaper-super-shallower-flight**: The super shuriken now costs half a bar — it needs 50 MOC and spends 50 instead of requiring a full bar and zeroing it — so a full bar fires two supers back to back and the 50–99 range is worth banking, while a press below 50 or with no visible enemy still fires a normal shuriken and keeps the MOC. The afterburner drains 25% faster at 12.5/s (a full 100 bar now sustains 8 seconds instead of 10), putting flight and the super shot in competition for the same fixed MOC pool. The bar's pulsing highlight reads silver at 50 and stays gold at 100, with nothing pulsing below 50, and both Polish menu hints were reworded to name the numbers; income rates, the 100 cap, the halving decay, and scoring are unchanged.
- **2026-09-28 — shuriken-charges-moc**: Shuriken kills now charge MOC at half the slam rate (`hpMax*5` per kill), so the free ground attack is a real income source for the bar alongside the risky slam. Only the shot that destroys the enemy pays out — hits on a surviving robot, and shurikens absorbed by a platform, charge nothing — and the super shuriken charges nothing at all, since it already spends the whole bar. The kill helpers' `slam` flag became a numeric `mul` multiplier (`1` slam, `0.5` shuriken, `0` super); the MOC scale, 100 cap, 10/s afterburner drain, halving decay, and HUD are all unchanged.
- **2026-09-28 — shuriken-key-to-q**: The shuriken fire key is now `Q` instead of `J`, with `X` kept as a second, interchangeable binding so the attack sits in the same hand cluster as movement; `J` is now completely unbound. Both Polish menu hints were reworded from `J/X` to `Q/X`, and the `moc-shuriken` spec now names `Q/X` as the super-shuriken key. Fire rate, shuriken speed, super-shot conditions, and every other control are unchanged.
- **2026-09-20 — ease-level-seven**: Level 7 is easier to clear: all 19 narrow floating platforms are widened from 38px to 48px for more forgiving landings, and the lone patrol hazard is lifted from y=310 to y=270 so it no longer scrapes the top of the peak platform where the player touches down (it still guards the crossing). No mechanics, timers, or other levels changed; the tier-2 robot checkpoint stays as-is.
- **2026-09-20 — add-doublejump-backflip**: The big double jump (midair Space) now plays a full 360° backflip over ~0.38s with smoothstep easing, rotating backward relative to travel in both facing directions. The flip is purely visual — collision box, velocity and slam arming are untouched — and is cleared immediately when the player lands or starts a slam. The small double jump (W/↑) stays upright, and no menu text changed.
- **2026-09-18 — add-moc-afterburner**: Added a MOC-powered afterburner: pressing the jump key midair with no air-jumps left ignites a rocket that lifts the player at a constant rise speed while held and hovers on release, draining MOC at 10/s (a full 100 bar sustains exactly 10 seconds). Flight grants immunity to enemy and hazard contact, and pressing ↓ mid-flight transitions into the existing slam dive, which still insta-kills and charges MOC. Landing, running out of MOC, or a slam automatically disengages the afterburner, rendered as a flickering orange flame with a matching Polish menu hint.
- **2026-09-15 — add-power-bar-slam**: Added a slam attack: after a midair Space jump, pressing ↓/S dives the player straight down, instantly killing any enemy on contact without taking damage. Slam kills charge the new MOC bar (0–100, +hpMax×10 per kill, reset on death and on level start), shown as a third HUD line and reserved for a future super-ability. Shurikens still work but do not charge MOC.
- **2026-09-15 — enemy-respawn-and-half-moc**: Falling into a pit now restores every hazard and robot to full health at its starting position, so the level can be cleared again after a respawn. MOC is no longer reset to 0 on a pit-fall, a loss retry, an R restart, or advancing to a new level — it is reduced by half (rounded down) on each transition and only a fresh run (new game from the menu or after finishing all levels) starts at 0.
- **2026-09-15 — add-moc-shuriken**: A full MOC bar lets the player fire a super shuriken (J/X) that homes onto the nearest on-screen enemy, passes through platforms, and instantly kills any enemy tier. Firing consumes the whole bar; a full bar with no visible target fires a normal shuriken and keeps the MOC. The super shuriken is rendered with a blue flame trail and core, with a matching Polish menu hint.