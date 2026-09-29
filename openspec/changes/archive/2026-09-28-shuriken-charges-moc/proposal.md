# Proposal

## Why

MOC is currently charged by exactly one thing: a slam kill. That makes the resource scarce by construction, and it splits the player's two attack routes into a "risky and slow but pays" route (slamming from a double jump) and a "free and repeatable but pays nothing" route (shurikens). The shuriken — the only attack usable from the ground, and the cheapest to land — contributes nothing to the bar that gates the game's single ultimate. Charging it at half the slam rate gives the resource a steady, low-risk income alongside the high-risk one, and turns MOC from a pure energy meter into a currency the player both earns and spends.

## What Changes

- A kill with a **normal shuriken** increases MOC by `hpMax*5` — half the slam rate (tier 1: +5, tier 2: +10, tier 3: +15, ghost: +10), capped at 100.
- Only the **killing** hit charges MOC. A shuriken that lands on a surviving robot reduces its health and awards points only; the charge arrives on the shot that finishes it. A shuriken consumed by a platform awards nothing.
- A **super shuriken** kill charges **no** MOC. It already consumes the entire bar, so the transaction nets out at zero and the bar stays a clean cost.
- MOC becomes a per-kill multiplier instead of a boolean: slam `1`, normal shuriken `0.5`, super shuriken `0`.
- MOC proportions, decay, and the HUD are untouched. The 100-point cap, the 10/s afterburner drain, halving on death and level change, and the gold pulse at full all stay exactly as they are.
- Non-goal: the shuriken is still free to fire and still limited only by the 0.35s cooldown. Firing itself never costs MOC — only the kill it lands pays.
- Non-goal: no menu or HUD change. The existing hints describe how to *spend* MOC and stay accurate.

## Capabilities

### New Capabilities

None. This adjusts existing behavior rather than introducing a new contract.

### Modified Capabilities

- `power-bar`: its "MOC charged by slam kills" requirement states `Shuriken kills MUST NOT charge MOC` and carries a scenario asserting exactly that. A `MODIFIED` delta cannot both keep that scenario and contradict it, so the requirement is `REMOVED` and replaced by "MOC charged by kills", which keeps the slam rate, adds the shuriken rate, and adds the killing-hit-only and no-cost-to-fire rules. `moc-shuriken`: its "One-hit kill" requirement defines what a super shuriken collision awards and is silent on MOC. The zero charge is stated explicitly so a future refactor of the shared kill helpers cannot silently make the ultimate self-funding.

## Impact

- `pizzaninja.html`: `killRobot(j,slam)` and `killHazard(j,slam)` (l. 118–119) take a multiplier instead of a boolean, and the four call sites pass it — `1` from the slam branches (l. 194, 209), `0.5` from the normal-shuriken branches, `0` from the super-shuriken branches (l. 253, 260). No new state, no assets, no rendering changes.
- `docs/changes.md` and `AGENTS.md`: changelog entries. The `AGENTS.md` mechanics line asserting that shurikens do not charge MOC becomes incorrect and needs rewriting.
- Player-facing: shuriken kills are now a MOC source, so a player who fires constantly banks the bar without ever slamming.
- Known follow-up, deliberately not addressed here: shuriken fire is not blocked during the afterburner, and the shuriken income rate (~14 MOC/s at full fire rate against tier-1 targets) exceeds the afterburner drain (10 MOC/s), so a skilled player in a robot-dense area can extend flight indefinitely. This change makes that reachable; the proportions and the fire guard are separate decisions.
