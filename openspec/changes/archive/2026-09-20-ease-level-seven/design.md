## Context

Motywacja: proposal.md — Why. Poziom 7 (`LEVELS[6]` w `pizzaninja.html`) to seria 27 skoków po przerwach 180 px między platformami 38 px, z **jednym** hazardem patrolującym tuż nad platformą szczytową (dolna krawędź hazardu `y+30 = 340` przylega do górnej krawędzi platformy `y=340`) i robotem tier 2 (2 trafienia) na platformie P9 o szerokości 70 px.

Formaty danych wg AGENTS.md:
- platformy: `[x, y, w, h]`; `h: 18` = floating (render: gałąź `h > 20`).
- hazardy: `[x, y, w, h, vx, min, max]`; patrol: `h.x` w zakresie `[min, max-h]`.

## Goals / Non-Goals

**Goals:**
- Złagodzić lądowania na całym poziomie 7 bez zmiany rytmu skoków (przerwy 180 px zostają).
- Usunąć hazard z pozycji „zapadki” na touchdownzie platformy szczytowej, zachowując groźbę przy nieostrożnym przeskoku.
- Minimalna powierzchnia zmiany: tylko literał `LEVELS[6]`, zero zmian w logice/renderze.

**Non-Goals:**
- Zmiana mechanik, timerów, robota (tier 2 na P9 zostaje), pozostałych poziomów (w tym 8), tekstów UI.
- Dodawanie/odejmowanie platform — geometria (x, y, przerwy) bez zmian.

## Decisions

**D1. Szerokość 38 → 48 px dla 19 platform floating (h: 18) poziomu 7.**
- Każda platforma wąska `[.., 38, 18]` → `[.., 48, 18]`. Przerwa efektywna 180 → 170 px; zakres lądowania 38 → 48 px.
- Alternatywa (odrzucona): zmiana `y` najwyższej platformy (320 → niżej) — zmienia rytm pionowych łuków, większy wpływ na odbiór poziomu niż szerokość.
- Alternatywa (odrzucona): dodanie platform bezpieczeństwa pod sekcjami — zmieniłoby liczbę/geometrię poziomu i strumień respawnów.

**D2. Hazard `y: 310` → `y: 270` (przesunięcie o 40 px w górę).**
- Nowy zakres pionowy hazardu `270..300` nie przecina już ciała gracza stojącego na platformie szczytowej `y=340` (gracz: `300..340`; `300 < 300` false → brak nakładki). Środek łuku przeskoku P6→P7 wciąż przechodzi przez pas `270..300` — hazard strzeże luki, ale przestaje trafiać przy samym lądowaniu.
- Konkretnie: `[1498,310,36,30,90,1408,1588]` → `[1498,270,36,30,90,1408,1588]` (wiersz 47). Patrol `min/max` bez zmian.
- Alternatywa (odrzucona): zawężenie patrolu `min/max` do samej luki — zmiana pozioma zmieniłaby moment interakcji z graczem; pionowa zmiana `y` jest mniej inwazyjna wizualnie (hazard nadal „strzeże” swojego piknaku).

**D3. Bez zmian dla robota oraz `time: 75`.**
- Robot tier 2 na P9 (2024..2094) pozostaje jedynym „checkpointem-wyzwaniem”; czas nie jest wąskim gardłem (podróż ~15s vs 75s).

## Risks / Trade-offs

- [Hazard przy `y=270` wciąż żądli na środku łuku — mniejszy margines niż dotychczasowe „ocieranie” o platformę] → Poziom zysku: touchdown przestaje wymagać idealnego wyczucia czasu; w razie potrzeby dalsze przesunięcie do `y=260` w testach.
- [Szerokość 48 px zmienia „odcisk” poziomu (38 px to sygnatura serii)] → Ryzyko estetyczne zaakceptowane przez rekomendację użytkownika; rytm 170-px przerw nadal wymagający.
- [Poziom 8 ma analogiczne ocierające hazardy (y=250 nad platformami 280)] → Świadomie poza zakresem; użytkownik prosił o poziom 7.