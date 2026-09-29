# Design

## Context

Every enemy death in the game funnels through two one-line helpers in `pizzaninja.html`:

```js
function killRobot(j,slam){const r=robots[j];robots.splice(j,1);score+=r.hpMax*50;msgTimer=0.8;msgText='+'+r.hpMax*50+'!;if(slam)power=Math.min(100,power+r.hpMax*10);}
function killHazard(j,slam){...if(slam)power=Math.min(100,power+h.hpMax*10);}
```

The `slam` parameter is not a mode selector — it is only ever read as a charge flag, and it is passed `1` from the two slam contact branches and left `undefined` from the shuriken branches. So "which weapon killed this" is already a single parameter at every call site, and adding a third value is a widening of an existing axis, not a new one.

Three constraints shape the approach: the codebase is one file with deliberately condensed style and no build step; the charge must not fire for a non-killing shuriken hit; and the helper must stay callable from the inside-outward loop that both already are.

See proposal.md for motivation, and `specs/` for the behavior being specified.

## Goals / Non-Goals

**Goals:**

- Express the charge as one number per kill source so the three routes (slam, normal shuriken, super shuriken) are legible at the call site.
- Leave the charging logic inside the shared helpers, so no branch in the update loop needs to know about MOC.
- Keep the diff to a handful of characters in `pizzaninja.html`, matching the file's existing density.

**Non-Goals:**

- Changing the MOC scale, the 100 cap, the afterburner drain, or the halving decay. Explicitly deferred by decision; this change must not touch them.
- Deciding what to do about shuriken fire during the afterburner. Recorded as a risk, not solved here.
- Any menu, HUD, or changelog-visible wording beyond the changelog entries themselves.

## Decisions

**The `slam` boolean becomes a numeric multiplier.** Call sites pass `1` (slam), `0.5` (normal shuriken), `0` (super shuriken); the helpers compute `power = Math.min(100, power + hpMax*10*mul)`.

Chosen over keeping the boolean and adding a second charge flag, which would need three call sites to know the full truth table, and over a string source (`'slam' | 'shur'`) that would require a lookup to express what is arithmetic. The multiplier keeps the existing `hpMax*10` expression intact and makes the shuriken rate a visible `0.5` rather than a duplicated constant.

The `1` in the existing slam call sites becomes the default: passing `0` explicitly for the super branch is what makes the zero charge readable at the source instead of relying on `undefined` being falsy — the current code's implicit contract is exactly the kind of thing that breaks silently on a refactor.

**The charge stays in the kill helpers, not in the hit branch.** The normal-shuriken branch at `pizzaninja.html:253` decrements health and calls `killRobot(j)` only when the robot dies, so the existing control flow already satisfies "only the killing hit charges" with no change to that branch — only the multiplier argument is added.

**Alternative considered: charge per landed hit instead of per kill.** Giving `+5` on every shuriken hit that connects is arithmetically equivalent to the chosen `hpMax*5` on the killing hit (a tier-N enemy takes N hits to destroy), so the totals match. Rejected: it would mean touching the hit branches to add charging logic, it would spread to the super shuriken's single hit and break the deliberate zero, and it would pay out for a hit on an enemy that a second shuriken was already going to finish. Per-kill keeps the payout in one place and makes the super shuriken's zero fall out of the same expression.

## Risks / Trade-offs

**Shuriken income can exceed the afterburner drain, and firing is not blocked in flight.** The fire guard at `pizzaninja.html:218` checks only the key and cooldown, not `p.ab` or `p.slam`. With `shootCd` 0.35 the maximum is ~2.86 shots/s, and a landed kill averages 5 MOC per shot against tier-1 targets — roughly 14.3 MOC/s against a 10 MOC/s drain, so a player hovering over a robot-dense stretch can extend flight past its intended 10 seconds. This change is what makes that reachable; today the income cannot be sustained at all. → Mitigation: none in this change, by decision. If it proves too strong, the narrowest fix is gating the fire guard on `!p.ab`, which closes the loop without touching proportions.

**Smaller increments lose more to the cap.** At 98 MOC a tier-1 kill contributes 2 instead of 5, where the slam's 10-point steps would have contributed 10. Over a full bar the loss is a few points across many small kills. → Accepted: the cap is a deliberate ceiling and the loss is invisible without instrumenting the bar.

**Two existing docs become wrong.** `AGENTS.md`'s mechanics note states that shurikens do not charge MOC, and the `power-bar` spec's Purpose still says "charged by slam kills". → Both updated as part of this change; the spec Purpose is edited in place at archive time per the delta workflow.

**Balance shifts toward the free attack.** MOC was previously reachable only by slamming, which requires a midair Space jump and positioning above a target; a player who never slams can now bank the bar with steady fire. → Accepted as the intent. The super shuriken stays rare because the per-level enemy count (1–4 robots plus one ghost) caps total obtainable MOC well below 100 for most levels, so the ceiling — not the rate — is what preserves the ultimate's scarcity.

## Migration Plan

Single-file change, no data or state migration. Rollback is reverting the multipliers to the previous boolean, which restores the old behavior exactly since the slam path keeps its rate of `hpMax*10`.

## Open Questions

- Whether the afterburner should block shuriken fire, or whether sustained flight should become a deliberate build. Deferrable: it changes neither the specs written here, nor the multiplier approach, nor the task breakdown.
- Whether the menu should advertise that shurikens charge MOC. The current hints describe only how to *spend* the bar and remain accurate, so this is cosmetic follow-up rather than a correctness gap.
