## Why

Po wpadnięciu w przepaść zabici wrogowie nie wracają na poziom — po wyczyszczeniu mapy nie ma czym napełniać paska MOC, a pełny reset mocy do 0 przy każdej śmierci i poziomie karze gracza jeszcze raz. Chcemy, żeby death/retry dawały drugą szansę na napełnienie mocy zamiast kasować postęp.

## What Changes

- **Enemy respawn na wpadnięcie w przepaść**: `respawn()` przebudowuje tablice `hazards`/`robots` ze stanu startowego poziomu (pełne HP, pozycje początkowe, patrole).
- **MOC zmniejszany o połowę zamiast zerowania**: zamiast `power=0` w `respawn()` i `beginPlay()` — `Math.floor(power/2)` w każdej zmianie próby/poziomu.
- **MOC trwa przez kampanię**: wygrana → nowy poziom startuje z połową poprzedniej mocy.
- **Świeży run nadal startuje z 0**: start gry z menu i po ukończeniu wszystkich poziomów (`done` → nowa gra) zeruje moc.

## Capabilities

### New Capabilities
- `enemy-respawn`: Wrogowie wracają do stanu startowego po wpadnięciu gracza w przepaść (i po retry/restarcie tego samego poziomu).

### Modified Capabilities
- `power-bar`: Wymaganie "MOC reset" zmienia się — moc nie zeruje się przy śmierci/retry/nowym poziomie, lecz zmniejsza o połowę (zaokrąglone w dół); zeruje się tylko przy świeżym runie.

## Impact

- `pizzaninja.html`: `respawn()` (l. 104), `beginPlay()` (l. 87–97), `startLevel()` (l. 80–86), wywołania klawisza R (l. 472) — wprowadzenie flagi "świeży run" ustalanej w `startLevel()` przed nadpisaniem `state`.
- Wyciągnięcie budowy enemies z `beginPlay()` do helpera `spawnEnemies()` współdzielonego przez `beginPlay()` i `respawn()`.
- `openspec/specs/power-bar/spec.md` — główny spec wymaga aktualizacji po zarchiwizowaniu (reguła MODIFIED).