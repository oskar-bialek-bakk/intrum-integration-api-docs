---
title: "ImportFromFileUpload"
---

**Opis:** Funkcja wykonuje import wybranego pliku

**Typ żądania:** POST

**URL żądania:** `https://[adres_api]/Dm/ImportFromFileUpload?importTypeId=16`

**Parametry żądania:**

| Nazwa | Typ danych | Typ parametru | Opis |
| --- | --- | --- | --- |
| importTypeId | int | query | Typ importu który chcemy wykonać |
| file | fromData | file | Plik który chcemy zaimportować |

**Pola odpowiedzi:**

Takie same jak w przypadku funkcji [GetImportStatus](get-import-status.md)
