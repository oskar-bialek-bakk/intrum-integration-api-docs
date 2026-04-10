# Integration API — Dokumentacja

Dokumentacja techniczna API integracyjnego DEBT Manager.

## Podgląd online

[https://oskar-bialek-bakk.github.io/intrum-integration-api-docs/](https://oskar-bialek-bakk.github.io/intrum-integration-api-docs/)

## Lokalne uruchomienie

```bash
pip install -r requirements.txt
mkdocs serve
```

Otwórz http://127.0.0.1:8000

## Sync do Confluence

```bash
cd sync
node sync-to-confluence.js          # sync wszystkiego
node sync-to-confluence.js ../docs/komunikaty/case.md  # sync jednego pliku
```

Wymaga pliku `sync/.env` z credentials (nie wersjonowany).

## Struktura

- `docs/` — źródłowe pliki Markdown
- `mkdocs.yml` — konfiguracja MkDocs Material
- `sync/` — skrypty synchronizacji z Confluence
- `requirements.txt` — zależności Pythona (MkDocs)
