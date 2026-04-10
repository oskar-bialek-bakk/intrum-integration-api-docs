---
title: "SetImportStep"
---

**Opis:** Funkcja ustawia aktualny krok importu

**Typ żądania:** POST

**URL żądania:** `https://[adres_api]/SetImportStep?importId=16`

**Parametry żądania:**

| Nazwa | Typ danych | Typ parametru | Opis |
| --- | --- | --- | --- |
| importId | GUID | query | ID importu o którego status odpytujemy |
| step | ImportStep | Body | Flaga informująca o tym czy pobierać szczegółowe informacje o statusie każdego komunikatu przetwarzanego w ramach importu |
| → Id | int | Body | Id kroku |
| → Added | Datetime | Body | Data dodania kroku |
| → Stage | int | Body | Etapy importy:Adding = 1,Transformation = 2,Validation = 3,Consumption = 4,Finishing = 5 |
| → StageStatus | int | Body | Status etapu:InProgress = 1,Done = 2,Error = 3 |
| → Message | string | Body | Komunikat powiązany z etapem i statusem np. na Etapie Validation i Statusie Error, może pojawić się opisowy komunikat błędu np. "Plik importowy zawiera błędy walidacji" |
| → StepDetailsList | złożony | Body | Lista szczegółowych statusów danego kroku np. na kroku walidacja lista zawierająca wszystkie błędy walidacji |
| → → Id | GUID | Body | Id szczegółowego statusu |
| → →Description | string | Body | Tekstowy opis szczegółowego statusu kroku np. "W linii 23 pliku importowego wystąpił błąd walidacji: brak kwoty wpłaty" |
| → →Type | int | Body | Typ szczegółowego statusu kroku:Info = 1,Warning = 2,Error = 3 |
| → → MatchKey | String | Body | Pole opcjonalne, pozwala powiązać szczegóły z konkretną encją w bazie danych (dzięki kombinacji pól MatchKey i MatchKeyType) |
| → → MatchKeyType | int | Body | Pole opcjonalne, pozwala powiązać szczegóły z konkretną encją w bazie danych (dzięki kombinacji pól MatchKey i MatchKeyType) |

**Pola odpowiedzi:**

Takie same jak w przypadku funkcji [GetImportStatus](get-import-status.md)

**Możliwe zmiany statusów:**

![Etapy importu](../../diagrams/import-stages-state.drawio)

| Stage | Wartość | Opis |
| --- | --- | --- |
| Adding | 1 | Dodawanie danych |
| Transformation | 2 | Transformacja danych |
| Validation | 3 | Walidacja danych |
| Consumption | 4 | Konsumpcja komunikatów |
| Finishing | 5 | Finalizacja importu |

| StageStatus | Wartość |
| --- | --- |
| InProgress | 1 |
| Done | 2 |
| Error | 3 |
