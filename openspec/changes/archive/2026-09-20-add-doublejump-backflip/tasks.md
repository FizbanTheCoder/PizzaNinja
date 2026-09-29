## 1. State

- [x] 1.1 Add `FLIP_TIME=0.38` module-level constant (near `player`, l.76) and `flip:0`, `flipF:1` fields to the `player` object literal
- [x] 1.2 Clear `flip` (and set `flipF`) in `respawn()`, `beginPlay()` reset blocks

## 2. Trigger

- [x] 2.1 In the jump-edge `airJumps>0` branch, arm the flip only when `big`: set `p.flip=FLIP_TIME` and `p.flipF=p.face`; small W/↑ double jump must not set it
- [x] 2.2 Clear `p.flip` on the `onGround` reset line (alongside `slam`/`slamOk`/`ab`)
- [x] 2.3 Clear `p.flip` when a slam starts (the `p.slam=true` branch)
- [x] 2.4 Decrement `p.flip` by `dt` each frame, clamped at 0 (alongside the `invuln` countdown)

## 3. Render

- [x] 3.1 In the player draw block, capture `const fc=p.flip>0?p.flipF:p.face` and use `fc` for the facing mirror
- [x] 3.2 After the mirror, when `p.flip>0` rotate by `-π*2*smoothstep(1-p.flip/FLIP_TIME)` around pivot `(13,20)`; smoothstep = `x*x*(3-2*x)`
- [x] 3.3 Verify in browser: big double jump flips 360° backward both directions with smoothstep easing, small double jump stays upright, slam/landing cut the flip, no residual rotation