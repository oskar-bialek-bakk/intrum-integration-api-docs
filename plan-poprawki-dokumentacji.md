# Plan poprawek dokumentacji API Integracyjnego

## Podsumowanie

Skonsolidowany plan poprawek dokumentacji — znalezione w audycie + zgłoszone przez użytkownika.
Każdy punkt ma severity i szacowany zakres (ile plików/ile pracy).

---

## CRITICAL

### C1. Zepsuty JSON w `case-contact-data.md`
- **Plik:** `docs/komunikaty/case-contact-data.md`
- **Problem:** Podwójny nawias `{`, brak przecinka po `"MatchKeyType": 6`
- **Zakres:** 1 plik, quick fix

### C2. Trailing comma w JSON w `payment.md`
- **Plik:** `docs/komunikaty/payment.md`
- **Problem:** `"PaymentTypeId": 15,` przed zamknięciem `}` — invalid JSON
- **Zakres:** 1 plik, quick fix

---

## MEDIUM

### M1. Zamiana "Intrum" → "Klient" (wersja bazowa)
- **Pliki:** `konfiguracja-kontrahenta.md` (2x), `architektura/index.md` (nazwy baz w diagramie i tabeli)
- **Cel:** Dokumentacja AS-IS jako wersja "wspólna" dla nowych klientów
- **Uwaga:** Nazwy baz `dm_integration_intrum`, `dm_data_dmweb_intrum` → zamienić na `dm_integration_[klient]`, `dm_data_dmweb_[klient]`
- **Zakres:** 2 pliki, ~10 zamian

### M2. Modernizacja stylu wizualnego
- **Pliki:** `mkdocs.yml`, `docs/stylesheets/extra.css`
- **Zakres pracy:**
  - **Fonty:** JetBrains Mono (code) + Source Sans 3 (text) via `theme.font` w mkdocs.yml
  - **Kolorystyka firmowa BAKK:** Dodać logo BAKK + dobrać paletę kolorów firmowych
  - **Logo w headerze:** `theme.logo` + `theme.favicon` w mkdocs.yml
- **NIE robimy:** Pełnych endpoint cards, method badges, custom JS — to byłby overkill w MkDocs Material. Wystarczą fonty + kolory + logo.
- **Zakres:** 2-3 pliki config/css, medium effort

### M3. Literówka "LOKALIZACA" → "LOKALIZACJA" w 14 komunikatach
- **Pliki:** Wszystkie 14 plików w `docs/komunikaty/` (oprócz `index.md`)
- **Problem:** Nagłówek kolumny w tabelce "Wykorzystanie pól komunikatu"
- **Zakres:** 14 plików, search-replace

### M4. Full-width content + lepsze tabelki
- **Pliki:** `docs/stylesheets/extra.css`
- **Problem:** `.md-content { max-width: none }` jest ustawione ale nie działa w pełni — MkDocs Material ma dodatkowe ograniczenia w `.md-grid`, `.md-main__inner`
- **Co zrobić:**
  - Override `.md-grid { max-width: 100% }` lub konkretne breakpointy
  - Tabele: `display: block; overflow-x: auto` z lepszym min-width na kolumny
  - Opcjonalnie wyłączyć sidebar na stronach z dużymi tabelkami
- **Zakres:** 1 plik CSS, medium effort
- **WAŻNE:** Zrobić PRZED M7, bo layout Kontomatik (opis + example side-by-side) wymaga szerokości

### M5. Diagramy — draw.io zamiast Mermaid
- **Pliki:** `konfiguracja-kontrahenta.md`, `procedura-importow.md`, `zalozenia/index.md`, `architektura/index.md`, `funkcje-api/importy/index.md`, `set-import-step.md`
- **Problem:** Diagramy Mermaid straciły podział na aktorów/swimlanes. Kolory z legendą `:blue_square:` się nie renderują.
- **Rozwiązanie:** Przerobić na draw.io z użyciem MCP pluginu. Draw.io wspiera swimlanes natywnie.
- **MkDocs integration:** Plugin `mkdocs-drawio` lub eksport draw.io → SVG/PNG i osadzenie
- **Zakres:** 6 diagramów, medium effort

### M6. URL-e w formacie kodu + zmiana wzorca adresu
- **Pliki:** 13 plików w `docs/funkcje-api/importy/`
- **Problem:** URL-e jako plain text, wzorzec `http://[serwer]:[port]/Dm/xxx`
- **Zamiana:** `` `https://[adres_api]/Dm/xxx` `` w inline code
- **Zakres:** 13 plików, search-replace + formatowanie

### M7. Modernizacja opisu parametrów (layout Kontomatik)
- **Pliki:** 13 plików w `docs/funkcje-api/importy/`
- **Problem:** Parametry w tabelkach z zagnieżdżonymi `→` — nieczytelne
- **Podejście:** Layout inspirowany Kontomatik/TransactionLink:
  - Opis tekstowy po lewej + JSON example po prawej (jeśli full-width z M4 działa)
  - POST: przykład JSON body z komentarzami
  - GET: parametry query jako lista + curl example
- **PILOTAŻ:** Najpierw przerobić JEDEN artykuł (np. `EnqueImportMessage`) — user potwierdzi, potem reszta
- **Fallback:** Jeśli side-by-side layout problematyczny → opis + JSON pod spodem (prostszy)
- **Zakres:** ~13 plików, HIGH effort (największy punkt planu)
- **Zależy od:** M4 (full-width)

### M8. Brakujące przykłady odpowiedzi (JSON)
- **Pliki:** `import-from-file-upload.md`, `import-from-file-share.md`, `get-import-file.md`, `get-import-file-from-file-share.md`, `refresh-dictionaries.md`, `set-import-step.md` + inne
- **Problem:** "Pola odpowiedzi: Takie same jak w przypadku funkcji" bez nazwy funkcji. Brak JSON example.
- **Co zrobić:** Dodać pełny JSON response example + uzupełnić brakujące nazwy funkcji
- **Zakres:** ~8 plików, medium-high effort
- **Realizować razem z M7** — jeden przebieg per endpoint

### M9. Broken cross-references (puste linki)
- **Pliki:**
  - `enque-import-message.md` — "patrz rozdział )" → link do Komunikaty
  - `import-from-file-share.md` — "Takie same jak w przypadku funkcji" → `GetImportStatus`
  - `import-from-file-upload.md` — j.w.
  - `set-import-step.md` — j.w.
  - `kolejki/index.md` — "pod następującym linkiem" → link do mssqltips.com
  - `kolejki/index.md` — zlepiona komórka tabeli (baza/schemat/tabela)
- **Zakres:** 5 plików, quick fix per plik

### M10. Nawigacja — duplikaty nazw sekcji i artykułów
- **Plik:** `mkdocs.yml` (nav)
- **Problem:** "Założenia" (sekcja) → "Założenia" (index.md) — mylące. Tak samo Funkcje API, Komunikaty, Przypadki użycia.
- **Rozwiązanie:** Użyć `navigation.indexes` (MkDocs Material feature — index pages stają się klikalne jako nagłówek sekcji, usunięcie duplikatu)
- **Zakres:** 1 plik (mkdocs.yml), ewentualnie 4 pliki frontmatter

---

## LOW

### L1. Literówka "strubg" → "string"
- **Pliki:** `get-import-status.md`, `get-object-status-by-id.md`
- **Zakres:** 2 pliki, quick fix

### L2. Zepsute formatowanie `Wyk****orzystanie`
- **Plik:** `attribute-debtor.md`
- **Zakres:** 1 plik, quick fix

### L3. Literówka "tektowy" → "tekstowy"
- **Plik:** `set-import-validations.md`
- **Zakres:** 1 plik, quick fix

### L4. Stub pages — minimalna treść
- **Pliki:** `funkcje-api/index.md`, `przypadki-uzycia/index.md`
- **Problem:** 1-2 linijki, brak przeglądu / TOC
- **Zakres:** 2 pliki, low effort

### L5. `refresh-dictionaries.md` — prawie pusty
- **Plik:** `docs/funkcje-api/importy/refresh-dictionaries.md`
- **Problem:** Brak opisu parametrów i odpowiedzi
- **Zakres:** 1 plik, medium (wymaga ustalenia struktury response)

---

## NICE-TO-HAVE

### N1. Macierz flag aktualizacyjnych (ObjectsUpdateBehaviour)
- **Plik:** `komunikaty/index.md`
- **Problem:** Duża tabela + legenda L1-L12, nieczytelne
- **Pomysły:**
  - Interaktywna tabela (JS + MkDocs)
  - Podział na mniejsze grupy (per komunikat)
  - Tooltip na hover z opisem flagi
  - Color-coded cells (zielone = dozwolone, szare = niedozwolone)
- **Zakres:** 1 plik, high effort, nice-to-have

---

## Sugerowana kolejność wykonania

1. **Quick wins (C1, C2, M3, M9, L1-L3)** — ~20 plików, szybkie fixy, natychmiastowa poprawa jakości
2. **M10 (nawigacja)** — 1 zmiana w mkdocs.yml, usunięcie duplikatów w menu
3. **M1 (Intrum → Klient)** — 2 pliki, search-replace
4. **M4 (full-width CSS)** — 1 plik CSS, poprawa dla wszystkich stron, **wymagane przed M7**
5. **M2 (styl: fonty + logo + kolory BAKK)** — CSS + config, wizualna poprawa
6. **M6 (URL-e)** — 13 plików, search-replace
7. **M5 (diagramy draw.io)** — 6 diagramów, draw.io MCP
8. **M7 pilotaż** — przerobić 1 endpoint (EnqueImportMessage) na layout Kontomatik, user potwierdza
9. **M7 + M8 (reszta endpointów)** — po potwierdzeniu pilotażu, ~13 plików
10. **L4-L5, N1** — stuby, macierz flag — na sam koniec
