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

```
