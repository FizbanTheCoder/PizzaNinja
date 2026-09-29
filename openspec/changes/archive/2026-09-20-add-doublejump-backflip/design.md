## Context

Single-file `pizzaninja.html`, no build system. The double jump happens in the jump-edge block of `update()` (pizzaninja.html:148–153): the `airJumps>0` branch consumes the flip the air-jump and sets velocity, while `big` (Space held) additionally arms `p.slamOk`. The player sprite is drawn relative to `(p.x,p.y)` (pizzaninja.html:438–473) with rect-based art spanning roughly x `-13..40`, y `-2..44`, and is mirrored for the left-facing case via `if(p.face<0)ctx.scale(-1,1)` (l.442). See proposal.md — Why; behavior contract in `specs/player-animation/spec.md`.

## Goals / Non-Goals

**Goals:**
- A read-back as a *backflip* (backward relative to travel) for both facing directions using one rotation formula.
- Zero effect on collision, physics or slam arming — animation only.
- Clean self-termination: no partial rotation ever persists.

**Non-Goals:**
- No new input, no new states, no menu/hint text, no changes to the afterburner or slam visuals.
- No refactor of the draw order beyond inserting the rotation.

## Decisions

**Timer field `p.flip` (seconds remaining), decremented in `update()` — mirrors the existing `p.invuln`/`shootCd` pattern (pizzaninja.html:183).** Triggered in the `airJumps>0` branch only when `big` (`p.flip=FLIP_TIME`), so the small W/↑ double jump keeps no flip (spec). Shared constant `FLIP_TIME=0.38` at module level (near `player`, l.76) because both `update()` and `draw()` need it — unlike the locals `SPEED/JUMP/GRAV` which live inside `update` only.

**Rotation applied AFTER the facing mirror, with a captured facing.**
```
save; translate(p.x,p.y);
if(p.slam) glow;
const fc = p.flip>0 ? p.flipF : p.face;
if(fc<0) scale(-1,1);                 // mirror
if(p.flip>0){ translate(PV); rotate(a); translate(-PV); }
...draw...
```
`a = -π*2*smoothstep(1 - p.flip/FLIP_TIME)` where `smoothstep(x)=x*x*(3-2*x)`. Because the rotation is inside the mirror, the same negative angle renders as a counterclockwise screen spin when facing right and its mirror (still backward-relative) when facing left — one formula, correct in both directions. `p.flipF` (facing captured at trigger) keeps the flip's spin direction from reversing if the player turns mid-air, which would read as a glitch.

**Pivot at the body center (~`PV=(13,20)` local) rather than the corner `(0,0)`.** A full 360° self-corrects at the end with any pivot, but mid-flip the corner pivot makes the sprite visibly orbit; the center pivot keeps the backflip tight around the body.

**Clears:** `p.flip=0` on the `onGround` reset line (l.146, alongside `slam`/`slamOk`/`ab`), on slam start (l.157), and in `respawn()`/`beginPlay()` reset blocks — covering every path the spec lists.

**Alternatives rejected:** time-based via the global `t` from `draw()` — accurate, but the codebase already tracks shorts with countdown fields (`invuln`, `shootCd`), so a seconds-remaining timer is the idiomatic fit. A pose-swap (no rotation) would read poorly for a "backflip".

## Risks / Trade-offs

- [Sprite visually overhangs its AABB corners mid-flip, and near a ceiling it may clip into a platform] → Cosmetic only, bounded by ~28–40px; consistent with the game's rect-art look, no physics change.
- [Turning directions mid-flip could flip the spin] → Captured `p.flipF` sidesteps it; costs one extra field.
- [Flip cut by an immediate slam may look abrupt] → Slam at `vy=1000` lasts a fraction of a second; the glow dominates, and the spec's clear-on-slam is the intended feel.