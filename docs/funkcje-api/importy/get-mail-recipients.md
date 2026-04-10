---
title: "GetMailRecipients"
---

**Opis:** Funkcja adresatów wiadomości e-mail, do których wysyłane są informacje o zakończeniu importów/eksportów

**Typ żądania:** GET

**URL żądania:** `https://[adres_api]/GetMailRecipients?importId=[]&exportId=[]`

**Parametry żądania:**

| Nazwa | Typ danych | Typ parametru | Opis |
| --- | --- | --- | --- |
| importId | guid | query (opcjonalny) | Id importu, dla którego pobieramy listę adresatów (brak przekazania wszystkich parametrów spowoduje pobranie pełnej listy adresatów) |
| exportId | guid | query (opcjonalny) | Id eksportu, dla którego pobieramy listę adresatów (brak przekazania wszystkich parametrów spowoduje pobranie pełnej listy adresatów) |

**Pola odpowiedzi:**

| Nazwa | Typ danych | Opis |
| --- | --- | --- |
| →Title | string | Tytuł wysyłanej wiadomości mail |
| →Recipients | string | Adresy e-mail adresatów (oddzielone średnikami) |
| →Agreement | string | Nazwa bazy, w kontekście której wysyłana jest wiadomość |
| →Creditor | string | Nazwa kontrahenta, w kontekście którego wysyłana jest wiadomość |
| →Operation | string | Nazwa importu/eksportu |
| →ExportTypeId | int? | Id słownika z tabeli export_typ |
| →ImportTypeId | int? | Id słownika z tabeli import_typ |
| →Link | string | Link prowadzący w aplikacji webowej do szczegółów importu/eksportu do podejrzenia szczegółów w systemie |

**Przykład odpowiedzi:**

```json
[
  {
    "Title": "t1",
    "Recipients": "test@bakk.com;test@gmail.com",
    "Agreement": "PKL",
    "Creditor": "PKO Leasing S.A.",
    "Operation": "PKL WYCOFANE",
    "ExportTypeId": null,
    "ImportTypeId": 12,
    "Link": "/user/blader/agreement-case/data-imports?caseId=703&creditorId=2&data=agreement&id=1"
  },
  {
    "Title": "t1",
    "Recipients": "test@bakk.com;test@gmail.com",
    "Agreement": "PKL",
    "Creditor": "PKO Leasing S.A.",
    "Operation": "PKO Leasing AkcjeTel dzienny",
    "ExportTypeId": 4,
    "ImportTypeId": null,
    "Link": "/user/blader/agreement-case/data-exports?caseId=703&creditorId=2&data=agreement&id=1"
  }
]
```
