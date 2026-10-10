# Kontekst projektu

Ten frontend (`index.html`) współpracuje z backendem `DonRubi/quant-desk-backend`.
Pełny kontekst projektu (decyzje, definicje rachunków, następne kroki, instrukcja testowania) jest w tamtym repozytorium:

- `docs/KONTEKST.md` (decyzje użytkownika, stan prac, następne kroki)
- `docs/TESTOWANIE.md`
- `docs/OCENA_WEJSCIA_KATALOG.md` (warunki oceny wejścia do wyboru)
- `docs/FORM_POWODY.md` (powody w Form jako mierzalne detektory – wdrożone)

Gałąź robocza w obu repozytoriach: `claude/eager-maxwell-iu3x1v` (nic nie jest scalone do `main`).
Plik `index.html` ma końce linii CRLF. Parametr testowy `?api=` podmienia adres backendu (tylko `localhost` i `*.onrender.com`).

## Moduły frontendu (skrypty na końcu `index.html`, globalne na `window`)
- `StockDetail` – okno spółki z wykresem, strefami, CVD (`.sd-*`).
- `Planer` – plany, strefy, zagrania, budżety (`.plx-*`); „Zrób zagranie” prowadzi do oceny wejścia.
- `Wejscie` – **Radar zagrań** (menu; filtr domyślnie do 5% od punktu wejścia, link TradingView) i okno **Ocena wejścia** (`.ev-*`); stąd „Dodaj transakcję →” wypełnia formularz Transakcji.
- `FormZones` – Form: zagrania pogrupowane po strefach, filtr wyciszonych, okno „Dodaj nowy wpis” (`.fz-*`).
- `Measure` – **powody mierzone** w oknie „Dodaj nowy wpis” (wykres 1D/1W, kotwice klikane w wykres, weryfikacja przez `/api/form/verify`, zapis w `reasons_detail`; `.fz-*`); formacje wymagają zapisanej wartości odniesienia.
- Notatnik na stronie głównej: opcjonalny **alert na Discord** (dzień i godzina w czasie polskim; `sql/home_notes_alert.sql` w backendzie, wysyłka przez `/api/cron/note-alerts`).
- Powody w Form mają **cykl życia** (luka pokryta = zakończona na stałe, OPD i Volume Profile trwałe); `FormZones.refresh()` odświeża je co 6 h.
- Transakcje nie wymagają już Focus: wybór „Zagranie z Form”, ocena wejścia zapisuje się przy transakcji (`entry_score`, `entry_eval`; `sql/entry_eval.sql`).
- Strażnik planu (`Planer`, szczegóły planu): stany stref, sygnały siły/słabości po zamknięciu sesji, dokładki (po sygnale siły / mechaniczna) → Ocena wejścia; plakietka jakości strefy w szczegółach Form (`FormZones.zoneQualityBlock`) i w Ocenie wejścia – informacyjna, nie wchodzi do Score; powód „Powtórzenie korekty" w `Measure`.
- Summary → zakładka **Audyt zasad (SL/TP)** (`loadRuleAudit`): zachowanie kursu po zakupach, wynik wg zasad vs faktyczny, drawdown przed zyskiem; tryb SL: zamknięcie dnia lub dotknięcie.
- Summary → **Symulator wyjść** (`loadExitSim`): macierz stop × TP z wynikiem ważonym, najgorszą transakcją i liczbą uruchomień; klik w komórkę pokazuje transakcje, w których reguła zadziałała.
- **Kartki planu** w Planerze (Strażnik planu): 🟨 strefa złamana, 🟧 OPD/SL złamany w spółce fundamentalnej (badanie: kolejne wejścia, limity, indeks/sektor/VIX), 🟥 spekulacyjna lub brak kolejnej strefy/limit; pole kluczowej świecy (SL) i ręczne potwierdzenie utrzymania; Serwis → odstępy do kolejnej strefy (`loadWatchSettings`). Spec: `docs/KARTKI_PLANU.md` w backendzie.
- **Planer automatyczny**: plan z pierwszej transakcji, wszystkie strefy z Form (liczba stref, limit/wykorzystanie na strefę, budżet spółki z polem „Maks. budżet spółki”), pole świecy SL w Form (dodawanie i szczegóły zagrania), pole „Maks. budżet spółki” w Focus (obowiązkowe przed wejściem).
- **Take Profit**: w szczegółach planu (Strażnik) linia TP1 i przycisk „Sugestie TP” (tabela celów: sufit, opory, szczyty, Volume Profile, strefy podaży; „Ustaw TP” zapisuje TP w Form); Ocena wejścia pokazuje źródło TP i SL.
- Podgląd konkretnej wersji bez cache: `https://raw.githack.com/DonRubi/trading-front-end/<hash commita>/index.html`.
