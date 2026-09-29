# Design: add-power-bar-slam

## Context

The whole game lives in a single `pizzaninja.html` file (960×540 canvas, condensed style, single-letter variables). The change touches four areas: jump logic, enemy loops in `update()`, resets (`respawn`/`beginPlay`), and the HUD in `draw()`.

## Decisions

### State

- `p.slamOk` (0/1) — slam armed. Set only in the midair-jump branch when `big` (Space). Cleared on `p.onGround` (alongside `p.airJumps=1`) and on every player reset.
- `p.slam` (0/1) — slam active (diving).
- `power` (0–100) — new global next to `score`.
- Constants: dive speed 1000 px/s, MOC charge `hpMax*10` per slam kill.

### Slam flow

```
midair Space jump (air jump, big=1)   → p.slamOk=1
↓/S while airborne, slamOk, invuln<=0 → p.slam=1, p.vy=1000
enemy contact during slam             → kill instead of damage
landing (onGround)                    → p.slam=0, p.slamOk=0
```

- In `update()`, the damage condition in the `hazards`/`robots` loops gains a branch: `if(p.slam && overlap) → kill`, otherwise the existing `p.invuln<=0 && overlap → damage`. The slam branch itself ignores invulnerability on contact, but activation requires `invuln<=0`.
- No bounce after a kill: `p.vy` stays high, the dive continues — kill chains come naturally.
- During slam the speed is pinned: `if(p.slam)p.vy=1000` runs after the gravity clamp (`vy>900`), so the dive holds a constant 1000 px/s.

### Shared kill path

- Robots: new `killRobot(j,slam)` — splice, `score+=hpMax*50`, message, and if `slam` then `power=Math.min(100,power+r.hpMax*10)`. Shurikens call it without the `slam` flag (no charging).
- Hazards: analogous `killHazard(j,slam)` with `+30` points and `+hpMax*10` MOC (hpMax=2).

### MOC resets

- `respawn()` (fall): `power=0`, plus `p.slam=0;p.slamOk=0`.
- `beginPlay()` (death/retry/new level): `power=0`, `p.slam=0;p.slamOk=0`.

### HUD

- Panel grows from 74 to 100 px tall (`fillRect(14,14,270,100)`).
- Third line: `MOC` label at (24,98) plus a 180×12 bar at (70,84): cyan frame, cyan→pink gradient fill proportional to `power/100`.
- At `power>=100`, gold pulsing (`shadowColor='#ffd24a'`, `shadowBlur` modulated by `Math.sin(t*8)`).

### Slam visual

- In the player draw: `if(p.slam)` → `ctx.shadowColor='#2de2e6';ctx.shadowBlur=18` while drawing the sprite.

### Menu

- Extra line under the existing instructions: `Szarża: podwójna spacja, potem ↓/S w powietrzu - ładuje MOC!` (existing lines shifted ~28 px up, ENTER stays at y=400).

## Risks / simplifications

- The slam also kills on side contact while diving — accepted (the dive is always straight down and fast).
- Diving into a pit = death and MOC reset — intended risk.
- `K['Space']` is cleared every frame in `loop()`; arming relies on the jump event, not key state — no impact.

## Verification

No test infrastructure exists; a headless Node harness stubs DOM/canvas and drives `update()`/`draw()` directly (24 assertions: arming, activation, insta-kills of robots and ghosts, no shuriken charging, invuln block, resets, cap at 100, rendering in all states).
