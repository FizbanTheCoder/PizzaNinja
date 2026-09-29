## Why

Pasek MOC był dotąd „tylko prezentacją" (zgodnie z opisem w main spec). Ta zmiana daje mu wreszcie super-zdolność, którą obiecuje menu: pełny pasek pozwala odpalić samonaprowadzający shuriken z niebieskim ogniem, który zabija dowolnego wroga jednym trafieniem. Domyka pętlę gry slam → MOC → super-zdolność.

## What Changes

- **Super shuriken z pełnego paska**: gdy `power>=100` i gracz naciska J/X, a na ekranie (w granicach kamery) jest wróg — zamiast zwykłego shurikena odpala się super shuriken, a `power` spada do 0.
- **Samonaprowadzanie**: super startuje w stronę najbliższego wroga (roboty + hazardy), co klatkę skręca do aktualnej pozycji celu; gdy cel zginął — retargetuje na najbliższego widocznego wroga, a gdy nie ma już celu — leci prosto.
- **Jedno trafienie = zabójstwo** dowolnego tieru robota i hazardu; punkty naliczane standardowo; super ginie na pierwszym kontakcie (bez przebijania).
- **Przebija platformy** — ginie tylko trafieniem wroga albo za krawędzią kamery.
- **Niebieska trała ognia** odróżnia super od zwykłego (cyjan) shurikena.
- **Bez marnowania paska**: gdy bar pełny, ale nie ma widocznego wroga — odpala się zwykły shuriken, a pasek zostaje nietknięty.
- Z `power-bar` znika klauzula „For now MOC has no use beyond presentation".

## Capabilities

### New Capabilities
- `moc-shuriken`: Super shuriken odpalany z pełnego paska MOC — warunki odpalenia, samonaprowadzanie, zabójcze trafienie, przejście przez platformy i prezentacja (niebieski ogień).

### Modified Capabilities
- `power-bar`: Wymaganie "MOC charged by slam kills" — usunięta klauzula "For now MOC has no use beyond presentation" (MOC zyskuje faktyczne zastosowanie).

## Impact

- `pizzaninja.html`: strzał (l. ~196–200), pętla shurikenów (l. ~201–226), rysowanie shurikenów (draw), HUD/menu (linia instrukcji o MOC), brak nowych zasobów.
- `openspec/specs/power-bar/spec.md` — po archiwizacji usunąć klauzulę "no use beyond presentation".