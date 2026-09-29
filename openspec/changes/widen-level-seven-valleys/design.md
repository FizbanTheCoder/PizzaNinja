# Design

## Context

See `proposal.md` — Why for the motivation. What shapes the approach is a single derived constraint, so it is worth stating precisely.

**The landing window is the platform width.** Jumping from platform A, the player lands on B when horizontal travel at the instant their feet cross `y_B` lies in `[gap, gap + width_B]`. Platform width is therefore the entire forgiveness budget, and nothing else in level 7 competes with it.

The physics constants (`update`, `pizzaninja.html:140`) are `SPEED=340`, `ACCEL=2600`, `JUMP=720`, `GRAV=1800`, with a player box of `w:28, h:40`. Derived:

- Big jump rise `= 720² / (2·1800) = 144 px`; air jump rise `= (720·0.85)² / (2·1800) = 104 px`.
- Airtime to return to takeoff height `= 2·720/1800 = 0.8 s` → `272 px` horizontal reach.
- Time to fall `dy` px below takeoff `= (720 + √(720² + 2·1800·dy)) / 1800`, so at full speed: **240–252 px** for a 40–60 px *ascent*, **290–298 px** for a 40–60 px *descent*.

Level 7 has 22 hops on a uniform 218 px pitch with 170 px gaps, vertical steps of 40–60 px, and 48 px pads — so the window closes at `170 + 48 = 218 px`. Every hop overshoots at full speed; the 9 descending hops overshoot by `+50` to `+80` px, the 12 ascending ones by `+22` to `+34` px. Air friction is only 600 px/s² in flight, so there is no cheap way to bleed speed once airborne — the player must modulate the input.

**Data format** (per `AGENTS.md`): platforms are `[x, y, w, h]`; `h: 18` is the floating case, `h: 60` the ground case, and rendering branches on `h > 20`. Ground platforms must not be widened casually. The level literal is `LEVELS[6]` — one line, `pizzaninja.html:45-48`.

## Goals / Non-Goals

**Goals:**
- Widen the landing window on the pads that carry the worst overshoot, without disturbing the 218 px pitch / 170 px gap rhythm that gives the level its character.
- Keep the change to the `LEVELS[6]` literal — zero edits to `update`, `draw`, collision, physics constants, HUD or UI text.
- Replace the arbitrary 70 px exception with a rule the level can be read by.

**Non-Goals:**
- No change to any `x`, `y` or `h`; no platforms added or removed.
- No changes to the hazard, the tier-2 robot on `P9`, `time: 75`, or any mechanic.
- No changes to levels 1–6 or 8. Level 8 (`LEVELS[7]`) is the same shape at 38 px and is harder than level 7 — explicitly out of scope, flagged in Risks.
- No new Polish UI text, so no menu hints change.

## Decisions

**D1. Target the `y=440` and `y=420` band — six platforms, not all nineteen.**

Three candidate sets were measured against the overshoot table:

| Set | Platforms | Descending hops fixed | Ascending hops fixed | Diff size |
|---|---|---|---|---|
| `y=440` + `y=420` (chosen) | 6 | 5 of 9 | 0 of 12 | 6 numbers |
| All descending landings | 9 | 9 of 9 | 0 of 12 | 9 numbers |
| All narrow pads | 19 | 9 of 9 | 12 of 12 | 19 numbers |

The widest set is the only one that eliminates the overshoot failure mode outright, since the 12 ascending hops overshoot by `+22` to `+34` px and need the same `+24` px to land at full speed. It was rejected as more change than was asked for and because a uniform 72 px across all 19 pads flattens the level's silhouette. The descending-only set was rejected as the worst of both worlds: a larger diff than the six-pad set while still leaving every ascent demanding, which makes the difficulty gradient incoherent rather than gentler.

The six-pad band was chosen because it is where the descending hops land — it captures 5 of the 9 overshoot landings, including all four deepest — and because it is the set the request named. Its cost is explicit and accepted: the 12 ascending hops and 4 descending hops keep their current windows, so the level gets meaningfully more forgiving at the valley floors without becoming easy overall.

**D2. Grow each pad to the right, leaving `x` untouched.**

The level is traversed strictly left to right, so the extra width is only useful on the leading edge — that is the edge the player arrives at. Anchoring right puts all 24 px of benefit in front of the player:

| Anchor | Resulting gaps | Landing room gained | Verdict |
|---|---|---|---|
| Right (`x` fixed) | uniform `146` / `170` / `178` | `+24` px, all of it forward | chosen |
| Centred | `153` / `158` / `162` / `170` | `+12` px forward, `+12` wasted behind | rejected |
| Left (grow backwards) | `170` + `194` | 0 px forward | rejected |

There is also precedent: the level's original 70 px pads were themselves built right-anchored — the gap before them is the standard 170 px and the gap after is 180 px, i.e. the author extended the right edge and let the following gap absorb it. Keeping `x` fixed also preserves the 218 px pitch exactly, so every platform stays on its original column.

**D3. Normalise `P9` and `P18` from 70 px to 72 px, not to 105 px.**

A literal +50% would take 70 → 105. That was rejected in favour of a uniform 72 px, which leaves the level with a legible rule — **valley floors 72 px, peaks 48 px** — instead of an unexplained 70.

This is the one place where the choice measurably costs difficulty, and it is recorded rather than buried. At 72 px the two hops that terminate on the already-wide pads gain almost nothing:

```
P8  -> P9   window [170,240] -> [170,242]   overshoot +54 -> +52 px
P17-> P18   window [170,240] -> [170,242]   overshoot +50 -> +48 px
```

At 105 px those would have been the *only* two hops in the level where the overshoot was essentially closed (`+19` and `+11` px). If playtesting shows those two landings still punishing, D3 is the first thing to revisit — it is a two-number change and requires no other edit.

**D4. Leave the vertical profile alone.**

Tempting to shorten the 40–60 px steps or drop the peaks, but vertical is already trivially non-binding: a 144 px rise against 40–60 px steps means the player clears every vertical with room to spare and even use the air jump. Horizontal modulation is the entire burden, so height is not a lever.

## Risks / Trade-offs

- **[The level stays demanding on ascents]** → Only 5 of 9 overshoot traps and 0 of 12 ascending hops are addressed. The change reduces the level's failure rate without removing its precision demand. If level 7 is still too hard after this, the next lever is widening the `y=400` pads (`P4`, `P17`) or the full 19-pad set — not more height changes.
- **[`P8→P9` and `P17→P18` barely improve]** → Consequence of D3. Mitigation: revert those two to 105 px, or move them to the `y=400` tier. Both are isolated numeric edits.
- **[Level 8 has the same shape at 38 px and is harder than level 7]** → 28 hops, 2 hazards, a tier-3 robot, 85 s. If level 7 lands well, level 8 will need at least the same treatment or more. Deliberately out of scope; a follow-up change is likely needed.
- **[The peak's zero-clearance stand-off is unchanged and fragile]** → The hazard on level 7 occupies `y 270..300`; a player standing on the peak `P7` (`y=340`) has their head at exactly `300`. `rectsOverlap` uses a strict `<`, so it does not fire, and the hazard's patrol maximum (1588) equals `P7`'s left edge so the two never overlap in x either. Safe by two independent one-pixel coincidences, both pre-existing and both untouched here. Worth knowing before anyone edits the hazard's `y` or `max` again.
- **[A visual "rest beat" is lost]** → `P9` and `P18` were the level's two conspicuously wide pads. At 72 px they are only 24 px wider than the norm, so the pause they signalled is subtler. Accepted as the price of the readable valley-floor rule.
