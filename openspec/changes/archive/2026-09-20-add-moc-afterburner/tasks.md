# Tasks: add-moc-afterburner

## 1. Core Physics

- [x] 1.1 Add `ab:false` to the `player` object literal
- [x] 1.2 In the jump-edge block (`jHeld&&!p.jumpHeld`) add ignition: `else if(p.airJumps===0&&!p.slam&&!p.ab&&power>0)p.ab=true`
- [x] 1.3 Replace the vertical-velocity lines so `p.slam` keeps `vy=1000`, `p.ab` sets `vy=-540` while the jump key is held or `0` while hovering, and only the non-flight player accumulates `GRAV`; clear `p.ab` in the `p.onGround` reset line
- [x] 1.4 Allow slam from flight: extend the down-key branch to `(p.slamOk||p.ab)` and clear `p.ab` when it starts
- [x] 1.5 Drain MOC while active: `power=Math.max(0,power-10*dt)`; disengage (`p.ab=false`) when `power<=0`
- [x] 1.6 Skip enemy/hazard contact damage and knockback while `p.ab` is on (keep the `p.slam` insta-kill branches first and unchanged)

## 2. Bounds and Interaction

- [x] 2.1 Clamp to the top of the play field while flying: `if(p.ab&&p.y<0){p.y=0;p.vy=0;}`
- [x] 2.2 Confirm existing behaviors still hold in `update`: double-jump unchanged, slam from `slamOk` unchanged, super shuriken unchanged, R restart / pit respawn halving unchanged

## 3. Presentation

- [x] 3.1 Draw an orange/yellow flickering flame under the player's feet when `p.ab` (distinct from cyan slam glow)
- [x] 3.2 Add a Polish menu hint (`Podwójne ↑ w powietrzu: dopalacz! 1 s = 1/10 MOC`) and shift the following hint lines to keep ENTER clear

## 4. Verification

- [x] 4.1 Headless Node harness (pattern from previous changes): ignition only when `airJumps==0` airborne (not on ground, not while `slam`); rise while jump held, hover on release; drain 10/s and auto-disengage at 0; ↓ starts a slam and clears `ab`; landing clears `ab`; no contact damage while flying; slam-from-flight still instant-kills and charges MOC; air-jump still takes precedence over ignition

## 5. Documentation

- [x] 5.1 Add a 2–3 sentence English entry to `docs/changes.md`
- [x] 5.2 Add the same entry to the "Change log" in `AGENTS.md`