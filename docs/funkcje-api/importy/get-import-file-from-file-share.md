---
title: "GetImportFileFromFileShare"
---

**Opis:** Funkcja kopiuje plik wybranego dla wybranego typu importu do lokalizacji na zasobie sieciowym, a następnie zwraca do niego ścieżkę

**Typ żądania:** GET

**URL żądania:** `https://[adres_api]/GetImportFileFromFileShare?ImportId=1538E757-F973-47B3-BD8A-76E384C25BDA`

**Parametry żądania:**

| Nazwa | Typ danych | Typ parametru | Opis |
| --- | --- | --- | --- |
| ImportId | GUID | query | ID importu |

**Pola odpowiedzi:**

| Nazwa | Typ danych | Opis |
| --- | --- | --- |
|  | string | Lokalizacja do pliku wybranego importu na zasobie sieciowym |
