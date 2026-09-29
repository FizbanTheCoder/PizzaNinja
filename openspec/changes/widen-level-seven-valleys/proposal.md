# Proposal

## Why

Level 7 (`LEVELS[6]`) is a 22-hop metronome: 23 platforms on a uniform 218 px pitch with 170 px gaps, and vertical steps of only 40–60 px against a 144 px big jump. Verticality is never the constraint — the sole difficulty currency is the horizontal landing window, and that window is *exactly* the platform width. A jump from platform A lands on B when horizontal travel at the moment the player's feet cross `y_B` falls in `[gap, gap + width]`.

At full run speed (340 px/s) an ascending hop travels 240–252 px and a descending hop travels 290–298 px, because falling covers the same ground over more airtime. With a 48 px platform the window closes at 218 px, so every hop in the level overshoots at full speed — and descending hops overshoot by 50–80 px, 2–3.6× worse than ascending ones. The descending hops are precisely the ones that land on the **lowest** platforms, which makes those six pads the level's most treacherous touchdown points. A player clearing the level must modulate horizontal speed continuously for its entire 5200 px length, and each mistake costs a pit-fall, a -5 s penalty and a return to the spawn at x=50.

## What Changes

- Widen the six lowest floating platforms of level 7 — the four at `y=440` and the two at `y=420` — to a uniform **72 px**, growing each one to the **right** (left `x` unchanged) so the extra width lands on the leading edge the player actually arrives at.
  - `P1  [280,440,48,18]` → `[280,440,72,18]`
  - `P5  [1152,440,48,18]` → `[1152,440,72,18]`
  - `P9  [2024,440,70,18]` → `[2024,440,72,18]`
  - `P14 [3146,420,48,18]` → `[3146,420,72,18]`
  - `P18 [4018,440,70,18]` → `[4018,440,72,18]`
  - `P21 [4704,420,48,18]` → `[4704,420,72,18]`
- Nothing else. No `x`, `y` or `h` values move; the 218 px pitch and 170 px gap rhythm are preserved exactly. `h: 18` is kept on all six, so the `h > 20` render branch is untouched.

Net effect on the six hops that terminate on these pads: the full-speed overshoot drops from `+24` to `0` px on the opening hop, and from `+72 / +76 / +80` to `+48 / +52 / +56` px on the four mid-level descents. The first jump of the level becomes landable at full speed with zero precision required.

Side effect, accepted deliberately: `P9` and `P18` were 70 px — the level's "rest beats", the only pads wider than the 48 px norm. Normalising them to 72 px retires that exception and leaves the level with a legible rule instead: **valley floors are 72 px, peaks are 48 px**. The cost is that those two hops (`P8→P9`, `P17→P18`) gain only 2 px of window and stay demanding; a literal +50% (70 → 105) was considered and rejected.

## Capabilities

### New Capabilities
_(none)_

### Modified Capabilities
_(none) — this is level content, not system behaviour._

Level 7 is data in the `LEVELS` literal, not a capability. All seven existing specs (`enemy-respawn`, `moc-afterburner`, `moc-shuriken`, `player-animation`, `power-bar`, `shuriken-controls`, `slam-attack`) describe mechanics and none constrain platform geometry, coordinates or widths. `skip_specs: true` is set in `.openspec.yaml`, following the precedent of `2026-09-20-ease-level-seven`.

## Impact

- `pizzaninja.html` — the `LEVELS[6]` literal only (platforms array, one line). No change to `update`, `draw`, collision, physics constants, HUD, or UI text.
- No overlaps introduced: the minimum resulting gap is 146 px. The tier-2 robot on `P9` (2040–2074, patrol 2024–2094) stays on the platform. The hazard (1408–1588) still never overlaps `P7` in x, so the peak's zero-clearance stand-off is unchanged.
- Level 8 (`time: 85`, `width: 6200`) deliberately untouched — it has the same shape at 38 px and is harder still.
- `docs/changes.md` and the "Change log" section of `AGENTS.md` get a 2–3 sentence entry each, per the project workflow.
