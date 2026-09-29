## Context

Single-file `pizzaninja.html`, no build system. Player physics live in `update(dt)` (pizzaninja.html:135–158): jump edges are tracked via `jHeld&&!p.jumpHeld`, jumps consume `p.airJumps` and the big-jump variant arms `p.slamOk`. The slam dive forces `vy=1000` and insta-kills on contact (pizzaninja.html:156–158, 188, 203). Enemy/hazard contact damage applies when `!p.slam && p.invuln<=0`. MOC (`power`, 0–100) is charged only by slam kills and spent by the super shuriken; the HUD bar (pizzaninja.html:467–474) already reflects the value. See proposal.md for motivation.

## Goals / Non-Goals

**Goals:**
- A third airborne lift option gated on `airJumps==0` with zero input-timer complexity.
- Climb/hover flight that drains MOC continuously, and a slam exit that reuses the existing slam kill/charge mechanics.
- Cheapest testable cost model (10 MOC/s, no minimum ignition).

**Non-Goals:**
- No new input timers or rebinding of the existing jump keys.
- No screen-shake, sounds, or new HUD element (bar + flame are the feedback).
- No platform pass-through (unlike the super shuriken) — ceilings bonk normally.
- No change to jump/double-jump constants or slam values.

## Decisions

**Ignition — reuse `airJumps` as the gate (no timer).**
In the jump-edge block, after the coyote/air-jump branches, add: `else if(p.airJumps===0&&!p.slam&&!p.ab&&power>0)p.ab=true;`.
- Why: `airJumps==0` already tracks "all land-based jumps spent", so the rocket is the natural rescue. No double-tap window, no new vars beyond `p.ab`.
- Alternative considered: a `upTapT` double-tap timer (needed suppression of the second jump; rejected as complexity).

**Flight physics — field `p.ab`, constant rise, hover.**
Replace the gravity accumulation for the airborne player:
```
if(p.slam)p.vy=1000;
else if(p.ab)p.vy=jHeld?-RISE:0;
else p.vy+=GRAV*dt;
```
with `RISE=-540` (between small jump −446 and big jump −720). Horizontal movement is untouched (A/D branch already independent of `vy`). Landing resets `p.ab=false` in the same line that resets `p.slam`/`p.airJumps` (pizzaninja.html:146).

**Slam from flight — allow `p.slam` without `slamOk`.**
The down-key branch (pizzaninja.html:156) becomes `if(!p.slam&&!p.onGround&&p.invuln<=0&&(down)&&(p.slamOk||p.ab)){p.slam=true;if(p.ab)p.ab=false;}`.
- Why: user chose slam priority; giving up the afterburner for a killing dive is the designed trade.

**MOC drain.**
When `p.ab`: `power=Math.max(0,power-10*dt)`; if `power<=0` set `p.ab=false`. Runs before drawing, so a full bar gives exactly 10 s of flight.

**Contact immunity while flying.**
In the hazard and robot overlap branches, skip the damage/knockback path when `p.ab` is on (keep the `p.slam` kill path first, unchanged). Immunity ends the moment the slam starts (temp flag flips), so the dive still kills.

**Top-of-screen clamp.**
After vertical movement: `if(p.ab&&p.y<0){p.y=0;p.vy=0;}` — the player stops at the top edge and keeps hovering (per spec).

**Presentation — orange flame, menu hint.**
Flame: 2–3 shrinking orange/yellow circles below the player's feet, flicker via `Math.sin(t*40)`, drawn when `p.ab` (distinct from cyan slam glow and blue super shuriken). Menu: one new Polish line + shift the two following hint lines down by 24 px so ENTER stays clear.

## Risks / Trade-offs

- [Flight trivializes the tight platform gaps in levels 7–8] → Limited by MOC: every second costs 10 and flying earns nothing itself (no slam contact = no charge), so sustained flight eats the super-shuriken budget. Keep 10/s flat for the first playtest and tune if it feels too cheap.
- [Air-jump + afterburner overlap on the same key press] → A single press never does both: the `airJumps>0` branch takes precedence, ignition only fires when `airJumps==0`.
- [Holding ↑ through a slam exit re-ignites] → Ignition requires `!p.slam`; slam is only cleared on landing, so no re-ignite mid-slide.
- [HUD has no explicit afterburner state] → The drain animation on the existing bar plus the flame under the player are the feedback; a HUD tag is a possible later tweak.

## Migration Plan

Single file, single edit pass. Rollback = revert the f() block in `update`, the `p.ab` field, the draw block, and the menu hint. No data migrations.