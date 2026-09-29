# Proposal: add-power-bar-slam

## Why

The game has only one way to kill enemies (the shuriken), which is safe and ranged. We want to reward risky close-quarters play: a new aerial slam attack that charges a MOC bar — the foundation for a future super-ability.

## What Changes

- New **slam** attack: after a midair Space jump, the player presses ↓ or S while airborne to dive straight down; any enemy touched (a robot of any tier or a ghost) dies instantly and the player takes no damage.
- Slam requires: a midair jump with **Space** (a midair W/↑ jump does not arm it), being airborne, and `invuln <= 0`.
- New **MOC** resource (0–100): charged only by slam kills (`+hpMax*10` per kill). Shurikens do not charge MOC. MOC has no use yet.
- MOC resets in `respawn()` (falling off) and `beginPlay()` (death/retry/new level).
- HUD gains a third line: the MOC bar (cyan, pulses gold when full).
- Player sprite gets a cyan glow while slamming.
- Menu screen gains a Polish instruction line for the slam.
- Project docs: `docs/changes.md` entry and an `AGENTS.md` change log entry.

## Capabilities

### New Capabilities
- `power-bar`: the MOC resource — charging via slam kills, 0–100 scale, reset on death and new level, HUD presentation.
- `slam-attack`: the slam attack — armed by a midair Space jump, activated by ↓/S while airborne at `invuln<=0`, insta-kills all enemies, no bounce after a kill.

### Modified Capabilities

_(none — no specs exist in openspec/specs/ yet)_

## Impact

- One file: `pizzaninja.html` (player state, enemy loops in `update()`, HUD in `draw()`, `respawn()`, `beginPlay()`, menu text).
- No changes to level data formats, no new dependencies.
- Menu instruction text (Polish) — updated.
- Docs: new `docs/changes.md`, "Workflow" and "Change log" sections in `AGENTS.md`.
