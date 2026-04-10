---
title: "SetImportValidations"
---

**Opis:** Funkcja uruchamia walidacje sprawdzane w ramach kroku walidacyjnego w imporcie

**Typ żądania:** POST

**URL żądania:** `https://[adres_api]/Dm/SetImportValidations`

**Parametry żądania:**

| Nazwa | Typ danych | Typ parametru | Opis |
| --- | --- | --- | --- |
| importId | GUID | Body | ID importu o którego status odpytujemy |
| step | ValidationRules | Body | Flaga informująca o tym czy pobierać szczegółowe informacje o statusie każdego komunikatu przetwarzanego w ramach importu |
| → Id | int | Body | Id warunku walidacyjnego z tabeli dm_config.validations |
| → Level | intInfo = 1Warning = 2Error = 3Ommit = 4 | Body | Opis zachowania walidacyjnego:Info - komunikat informacyjnyWarning - ostrzeżenie przed potencjalnymi problemamiError - Błąd walidacyjny - Wystarczy, że na dowolnej wiadomości zostanie zarejestrowany błąd walidacyjny i cały import nie jest wgrywanyOmmit - Zachowanie, które oznacza, że jeśli komunikat spełni tę regułę to jest odkładana o tym informacja (jak info/warning) oraz w imporcie danych do systemu komunikat, dla którego zostało zarejestrowane złamanie takiej reguły jest pomijany (pozostałe wczytywane są normalnie) |

**Pola odpowiedzi:**

Pusty napis w przypadku sukcesu, opis błędu (tekstowy) w przypadku błędu.

**Przykład:**



```json
{
  "ImportId": "10000000-0000-0000-0000-000000000000",
  "Rules": [
    {
      "Id": 1,
      "Level": 1
    }
  ]
}
```
