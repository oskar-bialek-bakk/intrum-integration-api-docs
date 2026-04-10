---
title: "GetObjectStatusByID"
---

**Opis:** Funkcja pobiera aktualny status danego obiektu na wybranej kolejce

**Typ żądania:** GET

**URL żądania:** `https://[adres_api]/Dm/GetObjectStatusByID?queueName=Customer&objectId=f40430d2-5c1e-4dff-8caf-f73af2f0301f`

**Parametry żądania:**

| Nazwa | Typ danych | Typ parametru | Opis |
| --- | --- | --- | --- |
| queueName | string | query | nazwa kolejki o którą status odpytujemy |
| objectId | GUID | query | ID obiektu o którego status odpytujemy |

**Pola odpowiedzi:**

| Nazwa | Typ danych | Opis |
| --- | --- | --- |
| QueueName | string | Nazwa kolejki na której będzie przetwarzany komunikat |
| Added | Datetime | Data dodania komunikatu do kolejki |
| ToDoAt | Datetime | Patrz rozdział → Budowa komunikatów |
| Processed | Datetime | Data zakończenia przetwarzania komunikatu. Jeśli ToDoAt to przyszła data to Processed pozostaje puste. |
| StatusId | int | Status przetwarzania komunikatu:0 = OFFLINE - komunikat jest ignorowany. Status wykorzystywany w momencie gdy plik importowy nie został jeszcze w całości rozbity na komunikaty i czekamy, aż pozostałe komunikaty zostaną wrzucone na kolejkę. 1 = QUEUED - komunikat na kolejce oczekuje na upłynięcie daty ToDoAt i pobranie do przetwarzania2 = SUCCESS - komunikat poprawnie przetworzony3 = ERROR - błąd przetwarzania komunikatu4 = REJECTED - komunikat odrzucony, nie podjęto próby przetwarzania5 = INPROGRESS - komunikat w trakcie przetwarzania |
| StatusMessage | string | Dodatkowy opis status np. dla komunikatów w statusie błąd pole zawiera opis błędu |
| Retries | int | Aktualna próba przetwarzania komunikatu |
| ObjectId | string | Patrz rozdział → Budowa komunikatów |
| MaxRetryCount | int | Maksymalna liczba prób przetwarzania komunikatu. Jeśli Retries = MaxRetryCount to komunikat nie zostanie już więcej przetworzony - wszystkie jego próby przetworzenia zakończyły się błędem. |

**Przykład odpowiedzi:**

```json
{
  "QueueName": "Customer",
  "Added": "2024-04-29T15:38:24.36",
  "ToDoAt": "2024-04-29T15:38:24.29",
  "Processed": "2024-04-29T15:38:30.663",
  "StatusId": 2,
  "StatusMessage": "",
  "Retries": 0,
  "ObjectId": "f40430d2-5c1e-4dff-8caf-f73af2f0301f",
  "VersionId": 18,
  "MaxRetryCount": 3
}
```
