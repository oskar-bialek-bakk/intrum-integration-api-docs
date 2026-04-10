---
title: "ImportFromFileShare"
---

**Opis:** Funkcja wykonuje import wybranego pliku z lokalizacji sieciowej

**Typ żądania:** POST

**URL żądania:** `https://[adres_api]/Dm/ImportFromFileShare?importTypeId=16&fileLocalization=ServerFiles%5CImport%5CSampkle108.xlsx`

**Parametry żądania:**

| Nazwa | Typ danych | Typ parametru | Opis |
| --- | --- | --- | --- |
| importTypeId | int | query | Typ importu który chcemy wykonać |
| fileLocalization | string | query | lokalizacja pliku na zasobie sieciowym |

**Pola odpowiedzi:**

Takie same jak w przypadku funkcji [GetImportStatus](get-import-status.md)
