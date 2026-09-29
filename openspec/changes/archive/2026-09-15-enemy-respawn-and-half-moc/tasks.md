# Tasks: enemy-respawn-and-half-moc

## 1. Enemy respawn

- [x] 1.1 Extract the hazards/robots instantiation from `beginPlay()` (l. 93–94) into `spawnEnemies()` and call it from `beginPlay()`
- [x] 1.2 Call `spawnEnemies()` from `respawn()` so a pit-fall restores all enemies to full health at starting positions
- [x] 1.3 In `respawn()`, keep everything else unchanged (player position, `invuln=1.5`, −5s, slam flags, no shuriken clearing)

## 2. MOC decay

- [x] 2.1 Add a module-level `freshRun` flag next to `power`; compute it in `startLevel()` as `state==='menu'||state==='done'` before any state mutation
- [x] 2.2 In `beginPlay()`, set `power = freshRun ? 0 : Math.floor(power/2)` (replaces `power=0`)
- [x] 2.3 In `respawn()`, set `power = Math.floor(power/2)` (replaces `power=0`)

## 3. Verification

- [x] 3.1 Headless Node harness (pattern from add-power-bar-slam): pit-fall respawn restores hazards+robots with no duplicates; MOC halves on pit-fall, loss-retry, R-restart and next level; fresh run (menu/done) starts at 0; rounding 1→0; shuriken charging unchanged

## 4. Documentation

- [x] 4.1 Add a 2–3 sentence English entry to `docs/changes.md`
- [x] 4.2 Add the same entry to the "Change log" in `AGENTS.md`