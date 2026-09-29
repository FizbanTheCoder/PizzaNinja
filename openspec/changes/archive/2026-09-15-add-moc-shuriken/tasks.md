# Tasks: add-moc-shuriken

## 1. Firing

- [x] 1.1 Add `enemyInView()` helper: nearest robot/hazard to the player whose rect overlaps the camera bounds (`cam.x..cam.x+W` x `0..H`), returns a live reference or null
- [x] 1.2 In the shoot block, compute `super` (full bar AND a visible enemy) and `power=0` when it fires; push shuriken with `vy`, `super`, `tgt`, `trail` fields (normal shots keep current behavior, `vy:0`)

## 2. Homing and hits

- [x] 2.1 Super update: steer velocity toward locked target at capped turn rate and fixed speed ~1.4× normal; re-lock via `enemyInView()` when `tgt` no longer exists (`robots.includes`/`hazards.includes`); straight line when no target
- [x] 2.2 Super ignores platform collisions; on enemy overlap call `killRobot(j)`/`killHazard(j)` directly (one-hit kill), consume the projectile on first contact
- [x] 2.3 Extend culling margin to the y-axis for super projectiles

## 3. Presentation

- [x] 3.1 Record a short trail of positions on the super shuriken; draw it as a fading blue flame trail and flame-blue core, distinct from the normal cyan shuriken
- [x] 3.2 Add a Polish menu line: `Pełna MOC: J/X - super shuriken!`

## 4. Verification

- [x] 4.1 Headless Node harness (pattern from previous changes): full bar + visible enemy fires super and zeroes MOC; full bar + no visible enemy fires normal and keeps MOC; bar<100 fires normal; super ultimately kills any tier and is consumed on first hit; passes platforms; no MOC charged by super kills; normal shuriken behavior unchanged

## 5. Documentation

- [x] 5.1 Add a 2–3 sentence English entry to `docs/changes.md`
- [x] 5.2 Add the same entry to the "Change log" in `AGENTS.md`