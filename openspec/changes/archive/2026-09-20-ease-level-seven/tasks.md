## 1. Edycja danych poziomu 7

- [x] 1.1 W `pizzaninja.html`, poziom z `time:75` (indeks 6): zmień szerokość wszystkich 19 wąskich platform (`[.., 38, 18]`) z `38` na `48` (platformy podstawy `h: 60` i szerokie `70` bez zmian)
- [x] 1.2 W tym samym poziomie: jedyny hazard `[1498,310,36,30,90,1408,1588]` → `[1498,270,...]` (y 310 → 270)

## 2. Weryfikacja

- [ ] 2.1 Otwórz `pizzaninja.html` w przeglądarce, przejdź do poziomu 7 (Enter) i potwierdź: platformy 48 px renderują się jako floating, hazard 270..300 nie trafia gracza stojącego na platformie szczytowej (`y=340`), robot tier 2 na P9 bez zmian
- [x] 2.2 Potwierdź, że żaden inny poziom (`time:85` itd.) nie został zmodyfikowany

## 3. Dokumentacja

- [x] 3.1 Dodaj wpis 2–3 zdaniowy do `docs/changes.md` (ENG) i sekcji "Change log" w `AGENTS.md` (ENG) opisujący złagodzenie poziomu 7