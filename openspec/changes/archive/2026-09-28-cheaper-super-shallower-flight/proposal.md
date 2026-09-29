# Proposal

## Why

Both MOC spends are behind gates that make them nearly theoretical. The super shuriken costs a full 100, but the total MOC obtainable on most levels falls well short of that — a run that never slams peaks around 78 — so the game's ultimate fires roughly once in a run, if at all. The afterburner's 10/s drain means a full bar buys 10 seconds of flight, so the bar is also barely worth spending on mobility. Both gates were set before shuriken kills started charging MOC, when the slam was the only income source and every point genuinely had to be scraped together. Now that a free attack feeds the bar, the income floor rose but the costs did not follow, so the resource reads as scarce while the player has no way to act on it. Halving the super shuriken to 50 and raising the drain to 12.5/s turns MOC from a hoarded resource into a live one.

## What Changes

- The super shuriken costs **50 MOC** instead of the full bar. It fires whenever MOC is at least 50, so a full 100 bar holds two supershot charges.
- Firing the super shuriken subtracts 50; it no longer empties the bar. A super shuriken kill still charges **no** MOC, so each shot nets −50.
- **The only gate is `power >= 50`.** There is no separate full-bar rule: a player sitting on 100 may fire two supershots back to back, and the second does not require dropping to 50 first.
- The afterburner drains **12.5 MOC/s** instead of 10, so a full bar sustains **8 seconds** of flight instead of 10. Ignition still requires any MOC above 0.
- MOC now has two competing live spends: 50 buys one supershot, and 50 buys 4 seconds of flight. Flying above a stretch can consume the material for the next supershot — the intended tension.
- **BREAKING**: the menu line `Pełna MOC: Q/X - super shuriken!` becomes false under the new threshold and is reworded to `Pół MOC (50): Q/X - super shuriken!`. The afterburner line `1 s = 1/10 MOC` becomes false and is reworded to `1 s = 12,5 MOC`.
- **BREAKING**: the MOC bar's "ready" pulse moves from 100 to 50, in silver. The existing gold pulse at 100 is kept and now means "two charges held" rather than "one super available".
- Non-goal: MOC income is unchanged — the slam stays at `hpMax*10` and shurikens at `hpMax*5`. The 100-point cap, the halving decay, scoring, and all controls are untouched.

## Capabilities

### New Capabilities

None. The super shuriken and the afterburner already exist; this lowers their cost.

### Modified Capabilities

- `moc-shuriken`: "Fired from a full MOC bar" is renamed to "Fired at half MOC" — its name asserts a full-bar condition that no longer exists. The threshold drops from 100 to 50, firing subtracts 50 instead of zeroing the bar, and a full bar now permits two shots in a row.
- `moc-afterburner`: "MOC consumption" changes from 10/s to 12.5/s, and the full-bar flight budget from 10 seconds to 8.
- `power-bar`: "MOC HUD presentation" currently requires a highlight only at 100. It gains a distinct highlight at 50 (silver, "can fire") alongside the gold at 100 ("holds two charges"), so the bar communicates the new threshold without text.

## Impact

- `pizzaninja.html`: the super-shot gate and spend (`pizzaninja.html:219`, `superShot=power>=100&&tgt` and `power=0` → `power>=50&&tgt` and `power-=50`), the afterburner drain (`pizzaninja.html:162`, `10*dt` → `12.5*dt`), the HUD pulse threshold and silver color (`pizzaninja.html:492`), and two Polish menu lines (`pizzaninja.html:301` and `341`). No new state, no assets, no rendering additions beyond a second shadow color.
- `docs/changes.md` and `AGENTS.md`: changelog entries. The `AGENTS.md` mechanics line stating the super costs the whole bar and the drain is 10/s must be rewritten, as must the afterburner's own "10 seconds" phrasing.
- Stale doc surfaced, deliberately not fixed here: `power-bar`'s spec Purpose still reads "the foundation for a future super-ability", which the already-archived `moc-shuriken` change invalidated. Per the delta workflow a spec Purpose is edited in place at archive time, so it is left for that pass.
- Player-facing: the super shuriken goes from a once-in-a-run flourish to a regular tool, and the afterburner from a 10-second to an 8-second burst. Anyone who learned to hoard MOC for a single super now has a resource they spend continuously.
- Known trade-off, accepted deliberately: the super shuriken is now the only action in the game that reduces MOC — the slam gains `hpMax*10` and three normal shurikens gain `hpMax*5`, while a super nets −50. It is best read as a cost paid for reliability (homing, no miss, passes platforms, kills any tier) rather than a reward for effort.
