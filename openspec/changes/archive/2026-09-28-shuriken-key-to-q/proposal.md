# Proposal

## Why

Shuriken is the only attack the player can fire on the ground, yet its binding (`J`/`X`) sits awkwardly under the right hand while the movement keys (`A`/`D` or the arrows) are already taken by the left. `J` also pulls the hand inward toward `W`/`S`, which serve the small jump and the slam. Moving the primary binding to `Q` — adjacent to `A`/`D` and the arrows, in the same "left hand" cluster as movement — makes the attack reachable without leaving the movement keys, and clears `J` from the key list entirely so the control surface stops advertising a duplicate.

## What Changes

- **BREAKING**: `J` no longer fires a shuriken. Pressing it has no gameplay effect.
- The shuriken fire key becomes `Q` (primary) with `X` retained as a secondary binding.
- The super shuriken follows the same binding: `Q`/`X` with a full MOC bar fires the super shuriken.
- The Polish menu hints are reworded from `J/X` to `Q/X` on both shuriken lines.
- Nothing else changes: fire rate (`shootCd` 0.35s), shuriken speed, super-shuriken homing, MOC charging, scoring, and every other control stay exactly as they are.

## Capabilities

### New Capabilities
<!-- Capabilities being introduced. Use kebab-case for path segments you introduce
     (e.g. user-auth or identity/user-auth) that follow the project's existing
     spec organization. Each creates specs/<capability-path>/spec.md. -->
- `shuriken-controls`: The player's shuriken fire binding — which keys throw, the cooldown between shots, and which keys are explicitly not bound. This contract was previously implicit in the code and only surfaced indirectly through the MOC super-shuriken requirement.

### Modified Capabilities
<!-- Existing capabilities whose REQUIREMENTS are changing (not just implementation).
     Only list here if spec-level behavior changes. Each needs a delta spec file.
     Use the exact existing path under openspec/specs/. Leave empty if no requirement
     changes. A change with no capabilities at all (pure refactor, tooling, docs)
     must set `skip_specs: true` in its .openspec.yaml - openspec validate rejects
     a zero-delta change without that marker. Do not invent a requirement just to
     satisfy validation. -->
- `moc-shuriken`: Its "Fired from a full MOC bar" requirement names `J/X` explicitly and names `J` in all three scenarios; the binding becomes `Q/X` and `J` is unbound.

## Impact

- `pizzaninja.html`: the shoot guard in `update()` (l. 218, `K['KeyJ']||K['KeyX']` → `K['KeyQ']||K['KeyX']`) and the two Polish menu lines (l. 338–339, `J/X` → `Q/X`). No new assets, no state, no rendering changes.
- `docs/changes.md` and `AGENTS.md`: changelog entries, plus the `AGENTS.md` Controls line that lists `J/X = shuriken`.
- Player-facing: anyone who muscle-memorized `J` for the shuriken must switch to `Q`; `X` still works.
