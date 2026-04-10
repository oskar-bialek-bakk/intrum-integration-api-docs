---
title: "GetDictionaries"
---

**Opis:** Funkcja pobiera wartości wszystkich słowników używanych przez Api

**Typ żądania:** GET

**URL żądania:** http://\[serwer\]:\[port\]/Dm/GetDictionaries

**Parametry żądania:**

| Nazwa | Typ danych | Typ parametru | Opis |
| --- | --- | --- | --- |

**Pola odpowiedzi:**

| Nazwa | Typ danych | Opis |
| --- | --- | --- |
| →Name | string | Nazwa tabeli słownikowej w postaci nazwa_schematu.nazwa_tabeli |
| →Rows | złożony | Lista wierszy występujących w słowniku |
| → →Id | int | Id wpisu z tabeli słownikowej |
| → →Name | string | Nazwa wpisu z tabeli słownikowej |

**Przykład odpowiedzi:**

```json

```
