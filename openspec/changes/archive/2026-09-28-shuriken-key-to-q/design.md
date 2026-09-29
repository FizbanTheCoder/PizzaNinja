# Design

## Context

See proposal.md for motivation. The relevant current state: the game's input is a single `K` map of `code → bool` filled by one `keydown`/`keyup` pair, so a binding is nothing more than a `code` string read at the point of use. The only shuriken consumer is the shoot guard in `update()` (pizzaninja.html:218), `if((K['KeyJ']||K['KeyX'])&&shootCd<=0)`, which then resolves normal-vs-super from the current MOC value and pushes onto `shurikens`. `shootCd=0.35` is set at the bottom of the same block and decremented once per frame above it, so the cooldown is already keyed off the shared guard rather than off any particular key.

This is a one-line code change. Design.md exists only to record why it is that one line and not a refactor.

## Goals / Non-Goals

**Goals:**
- Change the binding with the smallest possible diff: the key code in the guard, and the two Polish menu strings.
- Keep `Q` and `X` truly interchangeable, so the guard stays a flat OR over `code` values.
- Leave cooldown, shot resolution, rendering, scoring and every other control untouched.

**Non-Goals:**
- No input rebinding layer, keymap table, or settings screen.
- No remapping of movement, jump, slam, afterburner, or restart keys.
- No rebalancing — fire rate, shuriken speed, and the super-shot conditions are not up for review here.
- No change to how `J` was ever used elsewhere; it simply stops firing.

## Decisions

**1. Edit the key code inline in the existing guard; do not introduce a keymap constant.**

`K['KeyQ']||K['KeyX']` replaces `K['KeyJ']||K['KeyX']` at pizzaninja.html:218. A named constant (`const SHOT_KEYS=['KeyQ','KeyX']`) would be defensible in a codebase with many bindings, but this game has one fire binding out of ~8 controls and writes every other one inline (`K['ArrowLeft']`, `K['Space']`, `K['KeyR']`, …). Extracting a constant for the only case where two codes share a binding would be inconsistent with the surrounding style and would not be reused. Matches the repo convention of condensed inline checks (see AGENTS.md, Conventions).

Alternative considered: a `SHOT` alias (`K[SHOT]`) for readability. Rejected — no other control gets an alias, so it would read as an inconsistency rather than an improvement.

**2. Guard on `KeyQ` first, keeping `KeyX` as the retained secondary.**

Ordering is cosmetic (both sides of `||` are equally correct), but putting `Q` first matches the menu wording `Q/X - shuriken` and the new spec text, so the code reads in the same order as the player-facing hint.

**3. Menu strings: substitute `J/X` → `Q/X` in place, no reflow.**

Both lines already fit within the 960px canvas at `20px Courier New` and the neighbouring lines keep their `y` positions, so the substitution is a pure text edit. Widening `Q/X` to spell out `Q lub X` was rejected — the existing `J/X` shorthand sets the tone and the player has just read `A/D lub strzałki` on the line above.

**4. No debounce change for the both-keys-held case.**

Pressing `Q` and `X` together fires once and then re-arms after `shootCd`, because the guard is a single `if` evaluated once per frame against a shared `shootCd`. That already satisfies the "one shot per cooldown window" requirement; no extra guard is needed.

## Risks / Trade-offs

- [A player who muscle-memorized `J` for the shuriken finds it dead] → `X` is retained as an identical second binding, and the menu names both. This is the intended, accepted cost of the change (proposal, *What Changes*).
- [`J` silently does nothing, which can read as a dropped input rather than a removed key] → accepted; the menu never mentions `J`, and the game has no input-rejection feedback for unbound keys, so adding one would be a larger behavioral change than this one.
- [A future third shuriken key has no obvious home] → the flat OR in the guard still absorbs `||K['KeyY']` in one line; no structural debt is created.
