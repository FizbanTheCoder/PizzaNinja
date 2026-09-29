## Why

The big double jump (midair Space) has no visual payoff — the sprite just rises. A backflip sells the momentum and makes the double jump feel distinct from a plain jump, matching the game's snappy, cartoony feel.

## What Changes

- When the player performs the big double jump (midair Space), the sprite plays a full 360° backflip rotation over ~0.38s with smoothstep easing.
- The flip is purely visual: collision box, velocity and all game logic stay unchanged.
- The small double jump (W/↑) does NOT flip.
- Starting a slam or landing cancels/clears the flip so no residual rotation remains.
- No menu or hint text changes.

## Capabilities

### New Capabilities
- `player-animation`: visual animation states on the player sprite (this change introduces the backflip on the big double jump, as the first member of the capability)

### Modified Capabilities
<!-- None - no existing capability spec changes behavior. -->

## Impact

- `pizzaninja.html` — single-file game. Touches: `player` state object (one new timer field), the jump-edge branch in `update()` (trigger only in the `airJumps>0` branch when `big`), landing/slam/reset points (clear the timer), and the player draw block in `draw()` (rotation applied after the facing mirror). No physics, level data, or text changes.