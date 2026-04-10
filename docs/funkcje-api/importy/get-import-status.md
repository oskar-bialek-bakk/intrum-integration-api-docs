---
title: "GetImportStatus"
---

**Opis:** Funkcja pobiera aktualny status danego importu

**Typ żądania:** GET

**URL żądania:** http://\[serwer\]:\[port\]/Dm/GetImportStatus?importId=1&includeMessagesStatus=true

**Parametry żądania:**

| Nazwa | Typ danych | Typ parametru | Opis |
| --- | --- | --- | --- |
| importId | GUID | query | ID importu o którego status odpytujemy |
| includeMessagesStatus | boolean | query | Flaga informująca o tym czy pobierać szczegółowe informacje o statusie każdego komunikatu przetwarzanego w ramach importu |

**Pola odpowiedzi:**

| Nazwa | Typ danych | Opis |
| --- | --- | --- |
| ImportId | GUID | ID importu |
| ImportTypeName | string | Nazwa typu importu |
| DebtManagerImportId | int | ID importu w systemie DM (import.imp_id) |
| DebtManagerCreditorId | int | ID kontrahenta z systemu DM (kontrahent.ko_id) w ramach, którego wykonywany jest import |
| DebtManagerPortfolioId | int | ID portfela z systemu DM (umowa_kontrahent.uko_id) w ramach, którego wykonywany jest import |
| IsFinished | boolean | Flaga informująca czy import jest już zakończony |
| CurrentStep | złożony | Aktualny (ostatni) krok importu |
| → Id | int | Id kroku |
| → Added | Datetime | Data dodania kroku |
| → Stage | int | Etapy importy:Adding = 1,Transformation = 2,Validation = 3,Consumption = 4,Finishing = 5 |
| → StageStatus | int | Status etapu:InProgress = 1,Done = 2,Error = 3 |
| → Message | string | Komunikat powiązany z etapem i statusem np. na Etapie Validation i Statusie Error, może pojawić się opisowy komunikat błędu np. "Plik importowy zawiera błędy walidacji" |
| → StepDetailsList | złożony | Lista szczegółowych statusów danego kroku np. na kroku walidacja lista zawierająca wszystkie błędy walidacji |
| → → Id | GUID | Id szczegółowego statusu |
| → →Description | string | Tekstowy opis szczegółowego statusu kroku np. "W linii 23 pliku importowego wystąpił błąd walidacji: brak kwoty wpłaty" |
| → →Type | int | Typ szczegółowego statusu kroku:Info = 1,Warning = 2,Error = 3Ommit = 4 |
| StepsHistory | złożony | Historia wszystkich kroków importu. Każdy z kroków zawiera listę pól taką samą jak pola wskazane w polu CurrentStep. |
| ImportMessages | złożony | Lista wszystkich komunikatów na które został rozbity plik importowy wraz z ich ostatnim statusem przetwarzania. Pole uzupełnia się tylko jeśli parametr includeMessagesStatus = true |
| → QueueName | string | Nazwa kolejki na której będzie przetwarzany komunikat |
| → Added | Datetime | Data dodania komunikatu do kolejki |
| → ToDoAt | Datetime | Patrz rozdział → Budowa komunikatów |
| → Processed | Datetime | Data zakończenia przetwarzania komunikatu. Jeśli ToDoAt to przyszła data to Processed pozostaje puste. |
| → StatusId | int | Status przetwarzania komunikatu:0 = OFFLINE - komunikat jest ignorowany. Status wykorzystywany w momencie gdy plik importowy nie został jeszcze w całości rozbity na komunikaty i czekamy, aż pozostałe komunikaty zostaną wrzucone na kolejkę. 1 = QUEUED - komunikat na kolejce oczekuje na upłynięcie daty ToDoAt i pobranie do przetwarzania2 = SUCCESS - komunikat poprawnie przetworzony3 = ERROR - błąd przetwarzania komunikatu4 = REJECTED - komunikat odrzucony, nie podjęto próby przetwarzania5 = INPROGRESS - komunikat w trakcie przetwarzania |
| → StatusMessage | string | Dodatkowy opis status np. dla komunikatów w statusie błąd pole zawiera opis błędu |
| → Retries | int | Aktualna próba przetwarzania komunikatu |
| → ObjectId | strubg | Patrz rozdział → Budowa komunikatów |
| → MaxRetryCount | int | Maksymalna liczba prób przetwarzania komunikatu. Jeśli Retries = MaxRetryCount to komunikat nie zostanie już więcej przetworzony - wszystkie jego próby przetworzenia zakończyły się błędem. |
| ExternalFileName | string | Nazwa importowanego pliku |

**Przykład odpowiedzi:**

```json
{
  "ImportId": "2fa859e9-8479-4c7e-b1bb-c85f90f2402c",
  "ImportTypeName": "DM Integration API Sample - External Payments Import",
  "DebtManagerImportId": 3,
  "DebtManagerCreditorId": 10013,
  "DebtManagerPortfolioId": 94,
  "IsFinished": false,
  "CurrentStep": {
    "Id": 42,
    "Added": "2024-04-19T11:28:26.983",
    "Stage": 4,
    "StageStatus": 1,
    "Message": null,
    "StepDetailsList": []
  },
  "StepsHistory": [
    {
      "Id": 36,
      "Added": "2024-04-19T11:24:58.467",
      "Stage": 1,
      "StageStatus": 1,
      "Message": null,
      "StepDetailsList": []
    },
    {
      "Id": 37,
      "Added": "2024-04-19T11:24:58.607",
      "Stage": 1,
      "StageStatus": 2,
      "Message": null,
      "StepDetailsList": []
    },
    {
      "Id": 38,
      "Added": "2024-04-19T11:24:58.61",
      "Stage": 2,
      "StageStatus": 1,
      "Message": null,
      "StepDetailsList": []
    },
    {
      "Id": 39,
      "Added": "2024-04-19T11:24:59.493",
      "Stage": 2,
      "StageStatus": 2,
      "Message": null,
      "StepDetailsList": []
    },
    {
      "Id": 40,
      "Added": "2024-04-19T11:24:59.497",
      "Stage": 3,
      "StageStatus": 1,
      "Message": null,
      "StepDetailsList": []
    },
    {
      "Id": 41,
      "Added": "2024-04-19T11:28:26.98",
      "Stage": 3,
      "StageStatus": 2,
      "Message": null,
      "StepDetailsList": []
    },
    {
      "Id": 42,
      "Added": "2024-04-19T11:28:26.983",
      "Stage": 4,
      "StageStatus": 1,
      "Message": null,
      "StepDetailsList": []
    }
  ],
  "ImportMessages": [
    {
      "QueueName": "Case",
      "Added": "2024-04-19T11:28:26.91",
      "ToDoAt": "2024-04-19T11:28:26.87",
      "Processed": null,
      "StatusId": 5,
      "StatusMessage": null,
      "Retries": 0,
      "ObjectId": "bd459997-d8b3-4ae5-b00f-fe1a5042bd5a",
      "VersionId": 21,
      "MaxRetryCount": 3
    },
    {
      "QueueName": "Case",
      "Added": "2024-04-19T11:28:26.91",
      "ToDoAt": "2024-04-19T11:28:26.89",
      "Processed": null,
      "StatusId": 5,
      "StatusMessage": null,
      "Retries": 0,
      "ObjectId": "c97bef06-21ca-48ef-9352-9142cbee37e3",
      "VersionId": 22,
      "MaxRetryCount": 3
    },
    {
      "QueueName": "Contract",
      "Added": "2024-04-19T11:28:26.853",
      "ToDoAt": "2024-04-19T11:28:26.833",
      "Processed": "2024-04-19T11:28:37.82",
      "StatusId": 2,
      "StatusMessage": "",
      "Retries": 0,
      "ObjectId": "46a67979-e7c8-492f-a7a5-58cc9201f662",
      "VersionId": 20,
      "MaxRetryCount": 3
    },
    {
      "QueueName": "Contract",
      "Added": "2024-04-19T11:28:26.853",
      "ToDoAt": "2024-04-19T11:28:26.813",
      "Processed": "2024-04-19T11:28:37.82",
      "StatusId": 2,
      "StatusMessage": "",
      "Retries": 0,
      "ObjectId": "484a1987-08a3-4fbf-8749-0de3ac4c8716",
      "VersionId": 19,
      "MaxRetryCount": 3
    },
    {
      "QueueName": "Customer",
      "Added": "2024-04-19T11:28:26.78",
      "ToDoAt": "2024-04-19T11:24:59.913",
      "Processed": "2024-04-19T11:28:37.71",
      "StatusId": 2,
      "StatusMessage": "",
      "Retries": 0,
      "ObjectId": "1ea5daef-fe56-481d-bf1e-7bfb673790f2",
      "VersionId": 18,
      "MaxRetryCount": 3
    },
    {
      "QueueName": "Customer",
      "Added": "2024-04-19T11:28:26.78",
      "ToDoAt": "2024-04-19T11:24:59.893",
      "Processed": "2024-04-19T11:28:37.71",
      "StatusId": 2,
      "StatusMessage": "",
      "Retries": 0,
      "ObjectId": "80b1589a-d8ce-4f14-b222-76b90cefa938",
      "VersionId": 17,
      "MaxRetryCount": 3
    },
    {
      "QueueName": "Signature",
      "Added": "2024-04-19T11:28:26.943",
      "ToDoAt": "2024-04-19T11:28:26.93",
      "Processed": "2024-04-19T11:29:03.16",
      "StatusId": 3,
      "StatusMessage": "Case with match key: WRO00/W/WTSPR/9875841/749/23_10013_94 is not created",
      "Retries": 0,
      "ObjectId": "4d98879d-81e5-42dd-8649-656139603f0c",
      "VersionId": 24,
      "MaxRetryCount": 3
    },
    {
      "QueueName": "Signature",
      "Added": "2024-04-19T11:28:26.943",
      "ToDoAt": "2024-04-19T11:28:26.93",
      "Processed": "2024-04-19T11:29:03.16",
      "StatusId": 3,
      "StatusMessage": "Case with match key: WRO00/W/WTSPR/9875818/747/23_10013_94 is not created",
      "Retries": 0,
      "ObjectId": "f8c40d4b-ed93-4e36-9eba-478ef3e05c39",
      "VersionId": 23,
      "MaxRetryCount": 3
    }
  ],
  "ExternalFileName": "OkCaseSample.xlsx"
}
```
