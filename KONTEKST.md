# Kontekst projektu

Ten frontend (`index.html`) współpracuje z backendem `DonRubi/quant-desk-backend`.
Pełny kontekst projektu (decyzje, definicje rachunków, następne kroki, instrukcja testowania) jest w tamtym repozytorium:

- `docs/KONTEKST.md` (decyzje użytkownika, stan prac, następne kroki)
- `docs/TESTOWANIE.md`
- `docs/OCENA_WEJSCIA_KATALOG.md` (warunki oceny wejścia do wyboru)
- `docs/FORM_POWODY.md` (plan: powody w Form jako mierzalne detektory)

Gałąź robocza w obu repozytoriach: `claude/eager-maxwell-iu3x1v` (nic nie jest scalone do `main`).
Plik `index.html` ma końce linii CRLF. Parametr testowy `?api=` podmienia adres backendu (tylko `localhost` i `*.onrender.com`).

## Moduły frontendu (skrypty na końcu `index.html`, globalne na `window`)
- `StockDetail` – okno spółki z wykresem, strefami, CVD (`.sd-*`).
- `Planer` – plany, strefy, zagrania, budżety (`.plx-*`); „Zrób zagranie” prowadzi do oceny wejścia.
- `Wejscie` – **Radar zagrań** (menu; filtr domyślnie do 5% od punktu wejścia, link TradingView) i okno **Ocena wejścia** (`.ev-*`); stąd „Dodaj transakcję →” wypełnia formularz Transakcji.
- `FormZones` – Form: zagrania pogrupowane po strefach, filtr wyciszonych, okno „Dodaj nowy wpis” (`.fz-*`).
- Transakcje nie wymagają już Focus: wybór „Zagranie z Form”, ocena wejścia zapisuje się przy transakcji (`entry_score`, `entry_eval`; `sql/entry_eval.sql`).
- Podgląd konkretnej wersji bez cache: `https://raw.githack.com/DonRubi/trading-front-end/<hash commita>/index.html`.
