# Tasks: add-power-bar-slam

## 1. Slam state and logic

- [x] 1.1 Add `p.slamOk`/`p.slam` to the player object and a global `power=0` next to `score`
- [x] 1.2 Arming: in the midair-jump branch set `p.slamOk=1` only when `big` (Space); clear `p.slamOk` and `p.slam` on `p.onGround`
- [x] 1.3 Activation: ↓/S while airborne with `p.slamOk` and `p.invuln<=0` → `p.slam=1`, `p.vy=1000`; keep `vy>=1000` during the slam
- [x] 1.4 Branch in the `hazards`/`robots` loops: when `p.slam` and overlap → kill instead of damage; no bounce; landing ends the slam

## 2. MOC

- [x] 2.1 Extract `killRobot(j)`/`killHazard(j)` (splice + points + message) and use them for both kill sources; charge `power=min(100,power+hpMax*10)` only on slam kills
- [x] 2.2 Reset `power=0` (and slam flags) in `respawn()` and `beginPlay()`

## 3. Presentation

- [x] 3.1 HUD: grow the panel to 270×100, add the `MOC` line with a bar (cyan frame, cyan→pink fill per `power/100`, gold pulse at 100)
- [x] 3.2 Cyan glow on the player sprite during `p.slam`
- [x] 3.3 Menu: add a Polish slam instruction line

## 4. Verification

- [x] 4.1 Verify the slam per spec, MOC charging only from slams, resets on fall/death/new level, shurikens unchanged (headless Node harness, 24/24 assertions)

## 5. Documentation

- [x] 5.1 Create `docs/changes.md` with a 2–3 sentence English entry describing this change
- [x] 5.2 Add the same 2–3 sentence English entry to `AGENTS.md` (new "Change log" section)
- [x] 5.3 Record workflow conventions in `AGENTS.md`: all artifacts in English, `docs/changes.md` entry per change, explicit user consent required between planning artifacts and implementation
