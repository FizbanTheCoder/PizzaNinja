# Tasks

## 1. Super shuriken cost and threshold

- [x] 1.1 In `pizzaninja.html`, change the super-shot gate to `superShot=power>=50&&tgt` and the spend from `power=0` to `power-=50`. Verify a press at 100 fires twice and lands at 0, and a press at 50 leaves 0.
- [x] 1.2 Confirm no other code path zeroes `power` on firing, so the `power=0` removal does not affect the MOC cap or the decay sites. Verify by reading the shoot block end to end.

## 2. Afterburner drain

- [x] 2.1 Change the drain in the `p.ab` branch from `power-10*dt` to `power-12.5*dt`. Verify a full 100 bar now reaches 0 in 8 seconds and that the existing `power<=0` disengage still fires.

## 3. HUD thresholds

- [x] 3.1 Change the pulse gate to read silver at 50 or above and keep gold at 100, leaving both under-50 states unpulsed. Verify gold remains distinguishable from silver at a glance.
- [x] 3.2 Verify the bar fill still uses `power/100` unchanged, so 50 renders as a half-width fill with a silver pulse and 100 as a full-width fill with a gold pulse.

## 4. Polish menu text

- [x] 4.1 Reword the super-shuriken hint to name the 50-point threshold instead of a full bar, and the afterburner hint to name the 12.5 per second rate. Verify neither line contradicts the new numbers and the existing line spacing still fits the menu block.

## 5. Documentation

- [x] 5.1 Add a changelog entry to `docs/changes.md` in the existing style, covering the 50-point super, the 12.5/s drain and 8-second flight, the two-colour bar, and the reworded hints.
- [x] 5.2 Add the matching "Change log" entry to `AGENTS.md` and rewrite the mechanics lines that state the super costs the whole bar and the afterburner drains 10/s, including its "10 seconds" phrasing. Verify no line still describes the old costs.

## 6. Verification

- [x] 6.1 In the browser, confirm the bar pulses silver at 50, gold at 100, and not at all below 50, and that a super shot leaves exactly 50 less MOC.
- [x] 6.2 Confirm a full bar permits two super shots in a row with no cooldown restriction between them beyond the 0.35s fire cooldown, and that a super kill still awards no MOC.
- [x] 6.3 Confirm a full bar sustains 8 seconds of afterburner flight, that the afterburner disengages at 0, and that the unchanged behavior still holds: MOC income rates, the 100 cap, and the halving on pit-fall, retry, restart, and level advance.
