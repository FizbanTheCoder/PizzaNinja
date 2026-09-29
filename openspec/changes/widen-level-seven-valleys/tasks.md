# Tasks

## 1. Edycja danych poziomu 7

Wszystkie zmiany w jednej linii: `pizzaninja.html:46` (array `platforms` w `LEVELS[6]`).

- [x] 1.1 Poszerz cztery wąskie pady `48 px` na `72 px`, zmieniając **wyłącznie** slot szerokości i nie ruszając `x`, `y`, `h` (design D1 + D2, kotwica w prawo): `[280,440,48,18]` → `[280,440,72,18]`, `[1152,440,48,18]` → `[1152,440,72,18]`, `[3146,420,48,18]` → `[3146,420,72,18]`, `[4704,420,48,18]` → `[4704,420,72,18]`. Zweryfikuj przez ponowne wczytanie linii — cztery pary mają czytać się dokładnie tak.
- [x] 1.2 Znormalizuj dwa pady `70 px` do `72 px`, zmieniając wyłącznie slot szerokości (design D3): `[2024,440,70,18]` → `[2024,440,72,18]`, `[4018,440,70,18]` → `[4018,440,72,18]`. Zweryfikuj, że ich `x` (`2024`, `4018`) i `y` (`440`) pozostały nietknięte.
- [x] 1.3 Potwierdź, że edycja nie wykracza poza `platforms` w `LEVELS[6]`: niezmienione pozostają `time: 75`, `width: 5200`, `spawn`, `goal`, `hazards: [[1498,270,36,30,90,1408,1588]]` i `robots: [[2040,408,34,32,50,2024,2094,2]]`, a `LEVELS[0..5]` oraz `LEVELS[7]` nie mają żadnych zmian.
      **Uwaga — nie używaj `git diff` jako podstawy tego sprawdzenia.** Drzewo robocze jest brudne *przed* tą zmianą: `HEAD` (`1362c30 gra`) nie zawiera trzech zarchiwizowanych zmian (`ease-level-seven`, `add-doublejump-backflip`, `add-moc-*`, `shuriken-*`), więc `git diff` pokazuje ~31 hunków niezwiązanych z tą pracą. Zweryfikuj przez zrzutowanie kopii pliku **przed** edycją i porównanie jej z wersją po edycji (`diff` tych dwóch kopii ma pokazać różnicę wyłącznie w sześciu slotach szerokości na linii 46) — nie porównanie z `HEAD`.

## 2. Weryfikacja geometryczna

- [x] 2.1 Przelicz luki poziome ze zmienionego `LEVELS[6]` i potwierdź, że **żadne dwie platformy nie nachodzą się w osi x**, minimalna luka wynosi co najmniej `146 px`, a pitch `218 px` jest zachowany (żaden `x` nie przesunął się).
      Zweryfikowany wynik: brak nachodzeń, luka minimalna `146 px`, pitch nadal `{218, 250, 280}` (żaden `x` nie drgnął).
      **Korekta oczekiwania:** zestaw luk to `{146, 170, 178, 180}`, nie `{146, 170, 178}` — w pierwotnej treści zadania pominięto `180`, czyli lukę od ziemi startowej `P0` (kończy się na `100`) do `P1` (`280`), która istniała już przed zmianą. Zestaw luk przed zmianą: `{170, 180}`.
      Zmienione luki to dokładnie sześć poszerzonych padów: cztery `170 → 146` (`-24`, pady z 48 px) i dwa `180 → 178` (`-2`, pady z 70 px).
      Rozkład szerokości po zmianie: `72` × 6 (dna dolin), `48` × 15 (szczyty i środek), `100` × 1 i `180` × 1 (ziemia) — reguła „dna dolin 72 px, szczyty 48 px” obowiązuje.
- [x] 2.2 Potwierdź, że robot tier 2 na `P9` nadal mieści się na poszerzonym padzie oraz że hazard nigdy nie nachodzi na szczyt `P7` w osi x.
      Zweryfikowany wynik (zgodnie z semantyką ograniczeń patrolu w `update()`, gdzie `max` jest **prawą** krawędzią, a nie lewą):
      - robot: ślad `2024..2094` (lewa = `r.min`, prawa = `r.max`), pad `2024..2096` → **mieści się**, margines `2 px` z prawej. Przed zmianą pad kończył się na `2094`, więc robot był flush na obu końcach — poszerzenie daje mu dodatkowe `2 px`, nie jest regresją. Górna krawędź robota `440` = `y` pada, stoi na nim.
      - hazard: ślad `1408..1588`, `P7` = `1588..1636` → prawa krawędź hazardu `1588` **równa się** lewej krawędzi `P7`, więc brak nachodzenia w x na żadnej pozycji patrolu. (x `2040..2074`, patrol `2024..2094`) nadal mieści się w poszerzonym padzie (`2024..2096`), oraz że hazard (patrol `1408..1588`) nadal nigdy nie nachodzi na szczyt `P7` (`1588..1636`) w osi x. Zweryfikij porównaniem przedziałów — oba warunki to niezmienione zachowanie sprzed zmiany, więc wyłącznie potwierdź, że nie zostały naruszone.
- [ ] 2.3 Otwórz `pizzaninja.html` w przeglądarce, zacznij nową grę z menu i dotrzyj do poziomu 7 (Enter). Zweryfikuj, że sześć poszerzonych padów renderuje się jako platformy `floating` — gałąź renderująca `h > 20` nie może zostać wywołana, bo wszystkie sześć zachowuje `h: 18` — oraz że są wyraźnie szersze od 48-px szczytów.
      **Dowód kodowy (nie zastępuje oglądania):** `draw()` w `pizzaninja.html:355-364` — podstawa rysuje `fillRect(x,y,w,h)` plus 3-px cyanową krawędź górną, bez zależności od szerokości; gałąź dekoracyjnych cegiełek jest pod `if(h>20)` (linie 362-363). Wszystkie sześć poszerzonych padów ma `h: 18`, więc `18>20` = false i gałąź nie wejdzie — renderują się jak zwykłe pływające belki, tylko szersze. **Do weryfikacji wizualnej pozostaje przeglądarka.**
- [ ] 2.4 Przejdź pierwszy skok `P0→P1` z pełną prędkością, bez modulacji wejścia. Zweryfikaj, że gracz ląduje na `P1` — nadmiar tego skoku przy 340 px/s spada z `+24` do `0` px, więc lądowanie powinno być bezbłędne.
- [ ] 2.5 Przejdź wszystkie cztery poszerzone zejścia (`P4→P5`, `P13→P14`, `P17→P18`, `P20→P21`) i potwierdź, że każde jest do przejścia. Zweryfikuj przy okazji, **czy `P8→P9` i `P17→P18` wciąż są karalne** — te dwa skoki zyskują tylko `2 px` (design D3), więc ta obserwacja jest sygnałem do ewentualnego powrotu do decyzji D3.

## 3. Dokumentacja

- [x] 3.1 Dodaj wpis 2–3 zdaniowy do `docs/changes.md` pod nagłówkiem `## 2026-09-28 — widen-level-seven-valleys`, **na samej górze** (najnowsze wpisy idą najpierw, zaraz pod `# Change log`). Treść ma odnotować: sześć dolnych padów poziomu 7 (`y=440` i `y=420`) ma teraz `72 px` zamiast `48`/`70`; okno lądowania równe szerokości platformy było źródłem trudności tego poziomu, a poszerzenie usuwa nadmiar z pięciu najgorszych skoków zjazdowych; pady `70 px` zostały znormalizowane, więc obowiązuje reguła „dna dolin 72 px, szczyty 48 px”. Zaznacz, że mechaniki, timery i pozostałe poziomy pozostają bez zmian.
- [x] 3.2 Dodaj zgodny wpis 2–3 zdaniowy do sekcji "Change log" w `AGENTS.md` — na jej początku, tą samą datą i tą samą ramką co w 3.1.
- [x] 3.3 Zweryfikuj zmianę poleceniem `openspec validate widen-level-seven-valleys --type change` — oczekiwany wynik to czyste przejście z `skip_specs: true` i zerową deltą specyfikacji.
