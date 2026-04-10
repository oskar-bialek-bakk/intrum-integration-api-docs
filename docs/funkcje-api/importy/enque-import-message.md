---
title: "EnqueImportMessage"
---

**Opis:** Funkcja dodaje wiadomość na wybraną kolejkę

**Typ żądania:** POST

**URL żądania:** `https://[adres_api]/EnqueImportMessage`

**Parametry żądania:**

| Nazwa | Typ danych | Typ parametru | Opis |
| --- | --- | --- | --- |
| importId | Guid | Body | Id importu lub pusty GUID (00000000-0000-0000-0000-000000000000), jeżeli dodajmy na kolejkę wiadomość niepowiązaną z żadnym importem |
| queueName | string | Body | Nazwa kolejki do której zostanie dodana wiadomość |
| message | string | Body | lista obiektów w formacie JSON, która zostanie wrzucona na kolejkę (będzie się różnić w zależności od wybranej kolejki - patrz rozdział [Komunikaty](../../komunikaty/index.md)) |

**Pola odpowiedzi:**

| Nazwa | Typ danych | Opis |
| --- | --- | --- |
| Status | int | Status operacji:Success = 0,Warning = 1,Error = 2 |
| StatusName | string | Nazwa statusu operacji:Success,Warning,Error |
| StatusMessage | string | Tekstowy opis statusu Np. "W przekazanych komunikatach występują błędy walidacji" |
| StatusDetails | Lista | Lista szczegółów status np. "Błąd w ObjectID a0f22474-4cf1-4716-85a6-8ab84067b261: Brak uzupełnionego pola Waluta" |

**Przykład odpowiedzi:**

```json
{
  "Status": 0,
  "StatusMessage": null,
  "StatusDetails": [],
  "StatusName": "Success"
}
```
