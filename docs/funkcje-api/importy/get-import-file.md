---
title: "GetImportFile"
---

**Opis:** Funkcja zwraca plik importu na podstawie importId

**Typ żądania:** GET

**URL żądania:** `https://[adres_api]/Dm/GetImportFile?importId=43A79F4D-CBFE-4799-AE2A-48B68CD5EA97`

**Parametry żądania:**

| Nazwa | Typ danych | Typ parametru | Opis |
| --- | --- | --- | --- |
| importId | GUID | query | ID importu dla którego chcemy pobrać plik |

**Pola odpowiedzi:**

| Nazwa | Typ danych | Opis |
| --- | --- | --- |
|  | file | Plik importu |
