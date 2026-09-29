# Tasks

## 1. Fire binding

- [x] 1.1 In `update()`, change the shoot guard at pizzaninja.html:218 from `(K['KeyJ']||K['KeyX'])` to `(K['KeyQ']||K['KeyX'])`. Verify by reading the line back: `KeyJ` no longer appears anywhere in the file, and `Q` is checked before `X`.

## 2. Menu hints

- [x] 2.1 Change the menu line `Podwójny skok w powietrzu! J/X - shuriken` to use `Q/X` (pizzaninja.html:338). Verify both Polish shuriken lines now read `Q/X` and that neither menu line contains `J/X`; confirm the surrounding hint lines keep their existing `y` values so nothing overlaps.

## 3. Verification

- [x] 3.1 Headless check: extract the inline `<script>` from `pizzaninja.html`, stub `document`/`window`/`requestAnimationFrame`, drive the key handlers, and confirm: holding `Q` fires exactly one shuriken per 0.35 s cooldown; `X` fires identically; `J` fires nothing; `Q`+`X` together still fire only one; and with `power=100` plus a visible enemy, `Q` fires a super shuriken and zeroes MOC, while `X` still does too. Run with Node (`node -e '...'`) and do not commit the harness.
- [x] 3.2 Confirm no other control regressed: the file's only `Key*` reads are still movement, `Space`, `W`/`ArrowUp`, `ArrowDown`/`KeyS`, `KeyR`, and `Enter`.

## 4. Changelog

- [x] 4.1 Add a dated entry to `docs/changes.md` describing the binding change (Q primary, X retained, J removed, menu text updated).
- [x] 4.2 Add the matching entry to the "Change log" section of `AGENTS.md` and update its Controls bullet from `J/X = shuriken` to `Q/X = shuriken`.
