---
title: "GetImportStatus"
---

# GetImportStatus

<div class="endpoint-header">
  <div class="method-badge get">GET</div>
  <div class="endpoint-url">https://[adres_api]/GetImportStatus</div>
</div>

Pobiera aktualny status danego importu — bieżący krok, historię kroków oraz opcjonalnie statusy poszczególnych komunikatów.

---

<div class="api-section" markdown>
<div class="api-section-title">Request</div>

**Query parameters:**

<ul class="param-list">
  <li>
    <span class="param-name required">importId</span>
    <span class="param-type">Guid</span>
    <span class="param-desc">ID importu, o którego status odpytujemy</span>
  </li>
  <li>
    <span class="param-name">includeMessagesStatus</span>
    <span class="param-type">boolean</span>
    <span class="param-desc">Czy pobierać szczegółowe informacje o statusie każdego komunikatu przetwarzanego w ramach importu</span>
  </li>
</ul>

```bash title="Przykład wywołania"
curl "https://[adres_api]/GetImportStatus?importId=2fa859e9-8479-4c7e-b1bb-c85f90f2402c&includeMessagesStatus=true" \
  -H "Authorization: Bearer {TOKEN}"
```

</div>

---

<div class="api-section" markdown>
<div class="api-section-title">Response</div>

<ul class="param-list">
  <li>
    <span class="param-name">ImportId</span>
    <span class="param-type">Guid</span>
    <span class="param-desc">ID importu</span>
  </li>
  <li>
    <span class="param-name">ImportTypeName</span>
    <span class="param-type">string</span>
    <span class="param-desc">Nazwa typu importu</span>
  </li>
  <li>
    <span class="param-name">DebtManagerImportId</span>
    <span class="param-type">int</span>
    <span class="param-desc">ID importu w systemie DM (<code>import.imp_id</code>)</span>
  </li>
  <li>
    <span class="param-name">DebtManagerCreditorId</span>
    <span class="param-type">int</span>
    <span class="param-desc">ID kontrahenta z systemu DM (<code>kontrahent.ko_id</code>), w ramach którego wykonywany jest import</span>
  </li>
  <li>
    <span class="param-name">DebtManagerPortfolioId</span>
    <span class="param-type">int</span>
    <span class="param-desc">ID portfela z systemu DM (<code>umowa_kontrahent.uko_id</code>), w ramach którego wykonywany jest import</span>
  </li>
  <li>
    <span class="param-name">IsFinished</span>
    <span class="param-type">boolean</span>
    <span class="param-desc">Czy import jest już zakończony</span>
  </li>
  <li>
    <span class="param-name">ExternalFileName</span>
    <span class="param-type">string</span>
    <span class="param-desc">Nazwa importowanego pliku</span>
  </li>
  <li>
    <span class="param-name">CurrentStep</span>
    <span class="param-type">ImportStep</span>
    <span class="param-desc">Aktualny (ostatni) krok importu — struktura opisana poniżej</span>
  </li>
  <li>
    <span class="param-name">StepsHistory</span>
    <span class="param-type">ImportStep[]</span>
    <span class="param-desc">Historia wszystkich kroków importu (ta sama struktura co <code>CurrentStep</code>)</span>
  </li>
  <li>
    <span class="param-name">ImportMessages</span>
    <span class="param-type">ImportMessage[]</span>
    <span class="param-desc">Lista komunikatów z ich statusem przetwarzania. Wypełniane tylko gdy <code>includeMessagesStatus = true</code></span>
  </li>
</ul>

**Struktura `ImportStep`:**

<ul class="param-list">
  <li>
    <span class="param-name">Id</span>
    <span class="param-type">int</span>
    <span class="param-desc">ID kroku</span>
  </li>
  <li>
    <span class="param-name">Added</span>
    <span class="param-type">Datetime</span>
    <span class="param-desc">Data dodania kroku</span>
  </li>
  <li>
    <span class="param-name">Stage</span>
    <span class="param-type">int</span>
    <span class="param-desc">Etap importu:</span>
<ul class="status-values">
<li><code>1</code> — Adding</li>
<li><code>2</code> — Transformation</li>
<li><code>3</code> — Validation</li>
<li><code>4</code> — Consumption</li>
<li><code>5</code> — Finishing</li>
</ul>
  </li>
  <li>
    <span class="param-name">StageStatus</span>
    <span class="param-type">int</span>
    <span class="param-desc">Status etapu:</span>
<ul class="status-values">
<li><code>1</code> — InProgress</li>
<li><code>2</code> — Done</li>
<li><code>3</code> — Error</li>
</ul>
  </li>
  <li>
    <span class="param-name">Message</span>
    <span class="param-type">string</span>
    <span class="param-desc">Komunikat powiązany z etapem i statusem, np. na etapie Validation ze statusem Error: <code>"Plik importowy zawiera błędy walidacji"</code></span>
  </li>
  <li>
    <span class="param-name">StepDetailsList</span>
    <span class="param-type">StepDetail[]</span>
    <span class="param-desc">Lista szczegółowych statusów danego kroku (np. na kroku walidacja — lista błędów walidacji)</span>
  </li>
</ul>

**Struktura `StepDetail`:**

<ul class="param-list">
  <li>
    <span class="param-name">Id</span>
    <span class="param-type">Guid</span>
    <span class="param-desc">ID szczegółowego statusu</span>
  </li>
  <li>
    <span class="param-name">Description</span>
    <span class="param-type">string</span>
    <span class="param-desc">Tekstowy opis, np. <code>"W linii 23 pliku importowego wystąpił błąd walidacji: brak kwoty wpłaty"</code></span>
  </li>
  <li>
    <span class="param-name">Type</span>
    <span class="param-type">int</span>
    <span class="param-desc">Typ szczegółowego statusu:</span>
<ul class="status-values">
<li><code>1</code> — Info</li>
<li><code>2</code> — Warning</li>
<li><code>3</code> — Error</li>
<li><code>4</code> — Ommit</li>
</ul>
  </li>
</ul>

**Struktura `ImportMessage`:**

<ul class="param-list">
  <li>
    <span class="param-name">QueueName</span>
    <span class="param-type">string</span>
    <span class="param-desc">Nazwa kolejki, na której przetwarzany jest komunikat</span>
  </li>
  <li>
    <span class="param-name">Added</span>
    <span class="param-type">Datetime</span>
    <span class="param-desc">Data dodania komunikatu do kolejki</span>
  </li>
  <li>
    <span class="param-name">ToDoAt</span>
    <span class="param-type">Datetime</span>
    <span class="param-desc">Data, po której komunikat może zostać pobrany do przetwarzania</span>
  </li>
  <li>
    <span class="param-name">Processed</span>
    <span class="param-type">Datetime</span>
    <span class="param-desc">Data zakończenia przetwarzania. Puste, jeśli <code>ToDoAt</code> jest w przyszłości</span>
  </li>
  <li>
    <span class="param-name">StatusId</span>
    <span class="param-type">int</span>
    <span class="param-desc">Status przetwarzania komunikatu:</span>
<ul class="status-values">
<li><code>0</code> — OFFLINE — komunikat ignorowany (czeka na rozbicie pliku na komunikaty)</li>
<li><code>1</code> — QUEUED — oczekuje na upłynięcie <code>ToDoAt</code></li>
<li><code>2</code> — SUCCESS — poprawnie przetworzony</li>
<li><code>3</code> — ERROR — błąd przetwarzania</li>
<li><code>4</code> — REJECTED — odrzucony, nie podjęto próby przetwarzania</li>
<li><code>5</code> — INPROGRESS — w trakcie przetwarzania</li>
</ul>
  </li>
  <li>
    <span class="param-name">StatusMessage</span>
    <span class="param-type">string</span>
    <span class="param-desc">Dodatkowy opis statusu (np. dla ERROR — opis błędu)</span>
  </li>
  <li>
    <span class="param-name">Retries</span>
    <span class="param-type">int</span>
    <span class="param-desc">Aktualna próba przetwarzania komunikatu</span>
  </li>
  <li>
    <span class="param-name">ObjectId</span>
    <span class="param-type">string</span>
    <span class="param-desc">Identyfikator obiektu — patrz <a href="../../komunikaty/index.md">Komunikaty</a></span>
  </li>
  <li>
    <span class="param-name">MaxRetryCount</span>
    <span class="param-type">int</span>
    <span class="param-desc">Maksymalna liczba prób. Jeśli <code>Retries = MaxRetryCount</code>, komunikat nie będzie ponownie przetwarzany</span>
  </li>
</ul>

```json title="Przykład odpowiedzi"
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
    }
  ],
  "ExternalFileName": "OkCaseSample.xlsx"
}
```

!!! note "Skrócony przykład"
    Powyższy JSON zawiera skróconą historię kroków i komunikatów. W rzeczywistej odpowiedzi listy `StepsHistory` i `ImportMessages` mogą zawierać wiele elementów.

</div>
