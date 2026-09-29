# Design

## Context

Three numbers define the MOC economy, and all three live in single expressions in `pizzaninja.html`:

```js
pizzaninja.html:162   if(p.ab){power=Math.max(0,power-10*dt);...}          // afterburner drain
pizzaninja.html:219   const tgt=enemyInView(),superShot=power>=100&&tgt;
pizzaninja.html:221     power=0;                                           // super spend
pizzaninja.html:492   const bx=70,by=84,bw=180,full=power>=100;            // HUD pulse gate
```

The threshold at l.219 appears in two more places than the ones above: `moc-shuriken` names the full bar in a requirement title and three scenario names, and the Polish menu at l.301 advertises `Pełna MOC`. The drain rate appears once in code but three times in prose — the `moc-afterburner` spec text, its scenario title "A full bar lasts ten seconds", and the menu line `1 s = 1/10 MOC`.

That prose coupling is why this change is not a pure constant edit. Two of the three specs need their requirements replaced rather than modified: a scenario titled "A full bar lasts ten seconds" whose body says eight seconds is a self-contradicting document, and OpenSpec's archive refuses a MODIFIED that drops a scenario, so the honest move is REMOVED + ADDED with a migration note.

The income side is untouched and already specified: the slam grants `hpMax*10` and a normal shuriken `hpMax*5` on the killing hit only, both capped at 100.

See proposal.md for motivation and `specs/` for the required behavior.

## Goals / Non-Goals

**Goals:**

- Put the super shuriken and the afterburner in explicit competition for the same bar, so MOC is a resource the player spends continuously rather than hoards.
- Make the bar itself communicate the threshold: silver pulse from 50, gold at 100, so the player learns the rule without reading a number.
- Keep every number in one place, so the next rebalance touches three lines and two strings.

**Non-Goals:**

- Changing MOC income, the 100 cap, the halving decay, or scoring.
- Adding a charge counter or any new HUD element — the color change carries the information.
- Re-deriving `power-bar`'s spec Purpose, which a previously archived change already invalidated; per the delta workflow a Purpose is edited in place at archive time.

## Decisions

**The gate is `power >= 50` and nothing else.** The alternative — full bar still firing a "double shot" that empties the bar in one press — was rejected: it needs a second state in the fire guard, makes the gold pulse mean a different thing than "two charges held", and adds a rule the player must learn for no gain. One gate, one number.

**The spend is `power -= 50` rather than a set to a fixed value.** This falls out of the gate naturally: the bar already tracks charges as a quantity, so no new state is introduced and `100` means exactly "two charges remaining" with no interpretation.

**The HUD uses two pulse colors rather than a badge.** A digit at 100 would say "2", but it would also need a position, a font, and a rule for 1-versus-2 that the player still has to map to a keypress. Colors map to a state directly: silver means "you may fire", gold means "you may fire twice". The gold pulse already exists at l.492 as a shadow color and pulse expression, so gold is a threshold change and silver is a second branch beside it.

**Removed + added rather than modified, for both replaced requirements.** The alternative — keeping the old requirement names and editing bodies — would leave `moc-shuriken` with a title asserting a full-bar trigger that no longer exists and `moc-afterburner` with a scenario titled "ten seconds". The `RENAMED` header was considered for `moc-shuriken` and dropped: a rename plus a rewrite of every scenario is a replacement, and expressing it as one `REMOVED` + one `ADDED` keeps the reason and migration explicit in the delta itself.

**The menu states the number, not the effect.** `Pół MOC (50): Q/X - super shuriken!` rather than a vaguer phrasing, because the existing hint already teaches by number (`1 s = 1/10 MOC`) and the player needs the threshold to fire the right key. The competing option — `pełna MOC = 8 s lotu` — reads better but is a second number to keep in sync, and this change demonstrates the cost: the same line is edited in the same commit that moves the drain.

## Risks / Trade-offs

**Prose coupling to a number is now proven to be a maintenance cost.** Two of three specs had to be replaced rather than edited, purely because their scenario titles embedded the old rate. → Mitigation: none beyond what this change already does — it removes the second occurrence (`1/10 MOC`) in favor of the derived `12,5 MOC` and keeps thresholds in one place per spec. A future rate change repeats the exercise; that cost is accepted rather than solved, since solving it means dropping scenario titles or templating specs, both out of proportion to a one-file game.

**The super shuriken is the only action that decreases MOC.** The slam gains `hpMax*10`, three normal shurikens on a tier-3 gain `hpMax*5`, and a supershot nets −50 while achieving a one-hit kill that three shurikens also achieve. Read strictly, the ultimate is now worse value than its alternatives. → Accepted: the counter is reliability (homing, no miss, passes platforms, any tier) rather than economy, and making the ultimate common is the explicit goal. Flagged so a future rebalance is not surprised.

**MOC income does not scale with the higher spend rate.** The total obtainable per level is unchanged, so the shorter flight and cheaper super compete for the same fixed pool rather than a larger one. → Accepted as intended: the change redistributes a fixed resource between two spends and sharpens the choice. It does mean a player who leans on flight has less material for supershots, which is the tension the change is for.

**The `power-bar` Purpose remains stale until archive.** It still reads "the foundation for a future super-ability". → Left deliberately: the delta workflow edits a Purpose in place at archive time, and this change does not own that correction.
