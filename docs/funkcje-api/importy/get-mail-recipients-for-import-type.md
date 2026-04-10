---
title: "GetMailRecipientsForImportType"
---

**Opis:** Funkcja adresatów wiadomości e-mail, do których wysyłane są informacje o zakończeniu importów/eksportów

**Typ żądania:** GET

**URL żądania:** http://\[serwer\]:\[port\]/Dm/GetMailRecipientsForImportType?importTypeId=\[\]

**Parametry żądania:**

| Nazwa | Typ danych | Typ parametru | Opis |
| --- | --- | --- | --- |
| importTypeId | Int | query | Id typu importu, dla którego pobieramy listę adresatów |

**Pola odpowiedzi:**

| Nazwa | Typ danych | Opis |
| --- | --- | --- |
| →Title | string | Nieużywane |
| →Recipients | string | Adresy e-mail adresatów (oddzielone średnikami) |
| →Agreement | string | Nazwa bazy, w której znajduje się typ importu |
| →Creditor | string | Nazwa kontrahenta, w kontekście którego znajduje się typ importu |
| →Operation | string | Nazwa importu |
| →ExportTypeId | int? | Nieużywane |
| →ImportTypeId | int? | Id słownika z tabeli import_typ |
| →Link | string | Nieużywane |

**Przykład odpowiedzi:**

```json

```
