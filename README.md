# DEBT Manager — API Integracyjne

Dokumentacja techniczna API integracyjnego systemu **DEBT Manager** firmy [BAKK](https://bakk.com).

API umożliwia automatyczny import danych od kontrahentów, zarządzanie kolejkami komunikatów oraz monitorowanie statusu importów. Dokumentacja opisuje wszystkie dostępne endpointy, formaty komunikatów oraz praktyczne scenariusze użycia.

## Dokumentacja online

**[https://oskar-bialek-bakk.github.io/intrum-integration-api-docs/](https://oskar-bialek-bakk.github.io/intrum-integration-api-docs/)**

Strona budowana automatycznie z tego repozytorium (MkDocs Material + GitHub Pages).

## Zawartość

| Sekcja | Opis |
|--------|------|
| [Założenia](docs/zalozenia/) | Konfiguracja kontrahentów, procedura importów, architektura fizyczna, kolejki |
| [Funkcje API](docs/funkcje-api/) | 13 endpointów — importy, statusy, słowniki, walidacje, e-mail |
| [Komunikaty](docs/komunikaty/) | Specyfikacja 14 typów komunikatów (Case, Customer, Contract, Payment i inne) |
| [Przypadki użycia](docs/przypadki-uzycia/) | Praktyczne scenariusze: import z pliku, zasilenie bez pliku, nowy typ importu |

## Lokalne uruchomienie

```bash
pip install -r requirements.txt
mkdocs serve
```

Dokumentacja będzie dostępna pod `http://127.0.0.1:8000`.

## Struktura repozytorium

```
docs/                   Źródłowe pliki Markdown
  funkcje-api/importy/  Opisy endpointów API
  komunikaty/           Specyfikacje komunikatów
  przypadki-uzycia/     Przewodniki how-to
  diagrams/             Diagramy draw.io
  images/               Logo, zrzuty ekranu
  stylesheets/          Customowy CSS (branding BAKK)
sync/                   Skrypty synchronizacji z Confluence
mkdocs.yml              Konfiguracja MkDocs Material
requirements.txt        Zależności Pythona
```
