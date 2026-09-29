## Context

Single-file game (`pizzaninja.html`). Firing lives in the shoot block (l. ~196–200): `if((K['KeyJ']||K['KeyX'])&&shootCd<=0){ shurikens.push({x,y,vx,rot}); shootCd=0.35; }`. The update loop (l. ~201–226) moves shurikens horizontally (`s.x+=s.vx*dt`), culls off-camera (`cam.x-60..cam.x+W+60`), kills on platform overlap, and hits robots/hazards via `hp--` with `killRobot(j)`/`killHazard(j)` at `hp<=0`. `power` is a global charged by slam kills, capped at 100. See `proposal.md` — Why; behavior contract in `specs/moc-shuriken/spec.md`.

## Goals / Non-Goals

**Goals:**
- Overload the existing J/X fire: full bar + visible enemy → super shuriken, `power=0`; otherwise the shorthand normal behavior, bar untouched.
- Homing, one-hit-kill, platform pass-through, blue flame trail, all inside the existing `shurikens` array.

**Non-Goals:**
- No new meters, no piercing/chain kills, no cooldown changes (`shootCd` stays shared at 0.35s).
- No MOC charging from super kills (consumption only); points still awarded normally.
- No aim assist / auto-fire when the player isn't pressing J/X.

## Decisions

**1. Same array, flag-based branching.**
Extend the shuriken object with `vy:0, super:<bool>, tgt:<enemyRef>, trail:[]` (trail only for super). Fire decision computed in the existing push block:
```
const tgt=enemyInView();
const sp=power>=100&&tgt;
if(sp){power=0;}
shurikens.push({x:p.x+(p.face>0?p.w:-10),y:p.y+14,vx:sp?dirTo(tgt).x*SPEED_SUPER:p.face*560,vy:sp?dirTo(tgt).y*SPEED_SUPER:0,rot:0,super:sp,tgt:tgt||null,trail:sp?[]:null});
shootCd=0.35;
```
Rejected: a separate `supers[]` array — duplicates the update/draw loops for one flag. Rejected: holding super on a separate key — spec locks the input as J/X.

**2. `enemyInView()` — nearest visible enemy to the player.**
Scan `robots` + `hazards`; keep the closest to the player's center among those whose rect overlaps the camera rect `[cam.x, cam.x+W]×[0,H]`. Returns a live reference for `tgt`. Recompute after a kill so retargeting is "nearest *remaining* visible enemy" for free.

**3. Homing steering.**
Super missiles get `vy` and are steered each frame: recompute the unit vector to the locked target's center, then rotate the current velocity toward it at a capped turn rate (e.g. ~7 rad/s), maintaining speed ~1.4× normal (≈800). If `tgt` is no longer in `robots`/`hazards` (`!robots.includes(tgt)&&!hazards.includes(tgt)`), re-lock via `enemyInView()`; if that's null, keep current velocity (straight line).

**4. Platform pass-through + instant kill.**
In the shuriken loop: skip the platform-collision branch when `s.super`. In the enemy branches, when `s.super` call `killRobot(j)`/`killHazard(j)` directly (no `hp--`); set `hit`, break, splice — consumed on first contact. Points/messages come out of the existing kill functions.

**5. Culling for super.** Normal margin check stays. For `s.super` extend the same margin to the y axis (`cam.y` is implicitly 0..H), since homing can leave the view vertically.

**6. Blue flame trail rendering.** `s.trail` stores the last ~8 positions; drawn as shrinking, fading blue circles (`#3a6bff` etc.) behind the shuriken; the shuriken core is drawn in flame-blue with a strong glow. Normal shurikens keep the current cyan look. Distinguishable from `#2de2e6`.

**7. Menu hint.** Extend the Polish menu text (l. ~295) with a one-liner: `Pełna MOC: J/X - super shuriken!` so the loop slam → MOC → super is discoverable.

## Risks / Trade-offs

- [Locked target steers the missile fully — could feel "magnetic"] → capped turn rate + fixed speed; tune during implementation.
- [Player at full bar can't deliberately fire a normal shuriken] → accepted consequence of the overload; the fallback (normal when no visible enemy) limits the downside.
- [Missile mechanics add y-motion to a horizontally-only system] → all projectiles get `vy:0`; normal path ignores `vy`, so no behavior change there.
- [Spec statement "For now MOC has no use beyond presentation" becomes stale in main power-bar] → this change's delta already removes it; archive merges it.

## Migration Plan

Single-file game, no deploy pipeline. Change is additive within `shurikens[]`; rollback = revert the commit. No data migration.

## Open Questions

None that would change the specs or task breakdown. Tuning constants (speed, turn rate, trail length, flame colors) are implementation-time decisions.