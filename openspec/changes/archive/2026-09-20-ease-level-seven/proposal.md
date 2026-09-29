## Why

Poziom 7 (`LEVELS[6]`) jest praktycznie nieprzechodzalny: 27 skoków po przerwach 180 px na platformy szerokie tylko 38 px, a hazard patroluje dokładnie nad platformą szczytową (dolna krawędź hazardu przylega do górnej krawędzi platformy) — touchdown na niej obciąża gracza -3s i knockbackiem w przepaść, czyli powrotem na spawn i ponownym przebiegiem ~5000 px. Gracz po raz pierwszy przechodzący grę nie ma tu prawa do błędu.

## What Changes

- Zwiększenie szerokości wszystkich 19 wąskich platform (38 px, `h: 18`) poziomu 7 z `38` na `48` (lądowania bardziej wybaczające), z zachowaniem `h: 18` i tym samym gałęzi renderowania (floating).
- Jedyny hazard w poziomie uniesiony o 40 px pionowo: `y: 310` → `y: 270` (przestaje ocierać się o górną krawędź platformy szczytowej `y=340`, nadal grozi podczas przelotu/przeskoku w niewłaściwym momencie).
- Robot na platformie P9 (tier 2 = 2 trafienia) bez zmian.

Brak zmian czasowych (75s nie jest wąskim gardłem) i bez zmian mechanik. Poziom 8 (`time:85`) nietknięty.

## Capabilities

### New Capabilities
_(none)_

### Modified Capabilities
_(none — wyłącznie tunowanie danych poziomu; `skip_specs: true` w `.openspec.yaml`)_
<!-- Kapabilitakty nie zmieniają się: poziom 7 to treść (dane LEVELS), a nie zachowanie systemu. -->

## Impact

- `pizzaninja.html` — tylko literał `LEVELS[6]` (platformy, hazardy).
- Brak zmian w `update`, `draw`, mechanikach, HUD i tekstach UI.
- Zmiana szerokości `h: 18` nie dotyka gałęzi renderowania (`h > 20`).
- Aktualizacja `docs/changes.md` i "Change log" w AGENTS.md zgodnie z workflow.