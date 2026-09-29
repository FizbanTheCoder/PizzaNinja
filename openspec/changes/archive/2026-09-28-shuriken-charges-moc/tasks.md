# Tasks

## 1. Kill helper charge multiplier

- [x] 1.1 In `pizzaninja.html`, change `killRobot(j,slam)` to `killRobot(j,mul)` and the charge to `if(mul)power=Math.min(100,power+r.hpMax*10*mul)`, keeping the condensed one-line style. Verify the slam rate is unchanged by reading the expression back.
- [x] 1.2 Apply the same signature and charge change to `killHazard(j,mul)`, using `h.hpMax*10*mul`. Verify the ghost (always `hpMax` 2) yields 20 on a slam and 10 on a shuriken kill.
- [x] 1.3 Pass `1` at the two slam contact branches (`killHazard(i,1)` and `killRobot(i,1)`), leaving those call sites textually identical. Verify a midair slam still charges full rate.

## 2. Shuriken call sites

- [x] 2.1 Pass `0.5` from the normal-shuriken robot branch so a killing hit charges half (`killRobot(j,0.5)`), and leave the `r.hp--` non-kill path calling nothing. Verify a tier-3 robot takes 3 shots and charges only on the third.
- [x] 2.2 Pass `0.5` from the normal-shuriken hazard branch. Verify a ghost charges 10 total across its 2 shots.
- [x] 2.3 Pass `0` from both super-shuriken branches so the ultimate charges nothing despite spending the whole bar. Verify firing at a full bar still sets MOC to 0 and the kill does not raise it.

## 3. Documentation

- [x] 3.1 Add a `2026-09-28 — shuriken-charges-moc` entry to `docs/changes.md` in the existing two-or-three-sentence style, naming the half rate, the killing-hit-only rule, and the super shuriken's zero charge. Verify the entry sits at the top of the log.
- [x] 3.2 Add the matching "Change log" entry to `AGENTS.md`. Verify the existing MOC line stating that shurikens do not charge MOC is rewritten, not left contradicting the new behavior.

## 4. Verification

- [x] 4.1 Open `pizzaninja.html` in a browser and confirm with the on-screen bar: slam kills fill it at twice the rate of shuriken kills, non-killing shuriken hits do not move it, shurikens that hit platforms do not move it, and a super shuriken leaves it at 0.
- [x] 4.2 Confirm the untouched behavior: the 100 cap holds, the afterburner drains at 10/s, MOC still halves on a pit-fall, a retry, a restart, and a level advance, and firing a shuriken that hits nothing costs nothing.
