## Context

Single-file game (`pizzaninja.html`). `beginPlay()` (l. 87–97) rebuilds `hazards`/`robots` from `level.hazards`/`level.robots`; `respawn()` (l. 104–109) resets only the player and `power=0`, leaving enemies dead. `respawn()` is called from `update()` (l. 156) *before* the enemy loops (l. 161/176), so swapping those arrays inside it cannot corrupt the active iteration. See `proposal.md` — Why.

## Goals / Non-Goals

**Goals:**
- Enemies return to their starting state on pit-fall respawn.
- MOC halves (floor) on pit-fall, retry after loss, R-restart, and advancing to a new level; only a fresh run starts at 0.

**Non-Goals:**
- No change to score economy — respawned enemies still award full points (fall costs −5s).
- No per-enemy respawn timers, revive-in-place, or partial restores.
- No change to shuriken MOC charging.
- The future super-ability remains out of scope.

## Decisions

**1. `spawnEnemies()` helper.**
Extract the two `.map()` calls (l. 93–94) into `spawnEnemies()`, called from both `beginPlay()` and `respawn()`. One instantiation path, no divergence risk over time. Alternative considered: duplicating the maps in `respawn()` — rejected, two sources of truth.

**2. Module-level `freshRun` flag decided in `startLevel()`.**
`beginPlay()` runs either immediately (after a loss, `state='retry'`) or deferred behind the intro screen (Enter). `startLevel()` knows the caller state before mutating it, so it computes and stores `freshRun = (state==='menu'||state==='done')`. All other entries halve:
- menu/done → fresh → `power=0`
- win→next level, lose→retry, R restart → `power=Math.floor(power/2)`

`beginPlay()` then uses `power = freshRun ? 0 : Math.floor(power/2)`. Alternative considered: passing a param through the intro-Enter indirection — rejected, the flag is simpler and covers both entry paths.

**3. Rounding: `Math.floor(power/2)`.**
1 → 0 (never freezes MOC at 1). `Math.round` would keep 1 forever.

**4. `respawn()` calls `spawnEnemies()` + `power=Math.floor(power/2)`.**
Everything else in it (`invuln=1.5`, −5s, slam flags, position) is unchanged.

## Risks / Trade-offs

- [Score farming via repeated pit-falls] → each fall costs −5s and time feeds the win bonus; revisit if it becomes exploitable.
- [R-restart becomes a "cheap halve" escape] → halving is always a net loss for MOC and costs nothing extra (no timer penalty), low incentive; accepted.
- [In-flight shurikens survive a respawn and may hit freshly spawned enemies] → minor, acceptable; `respawn()` does not clear `shurikens` and this change does not alter that.
- [Name change `MOC reset` → `MOC decay on death or level change` requires careful archive merge] → the delta carries RENAMED + MODIFIED; at sync time apply the rename first, then the content, and verify the other `power-bar` requirements intact.