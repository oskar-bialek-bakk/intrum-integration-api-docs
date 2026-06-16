---
title: "GetImportStatus"
---

# GetImportStatus

<div class="endpoint-header">
  <div class="method-badge get">GET</div>
  <div class="endpoint-url">https://dmapi-intrum-dev.groupad1.com/pl/IntegrationsAPI/import/GetImportStatus</div>
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
curl "https://dmapi-intrum-dev.groupad1.com/pl/IntegrationsAPI/import/GetImportStatus?importId=2fa859e9-8479-4c7e-b1bb-c85f90f2402c&includeMessagesStatus=true" \
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
    <span class="param-desc">Nazwa pliku źródłowego importu — pole historyczne; w integracjach API-2-API zwykle <code>null</code>.</span>
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
    <span class="param-name">id</span>
    <span class="param-type">int</span>
    <span class="param-desc">ID kroku</span>
  </li>
  <li>
    <span class="param-name">added</span>
    <span class="param-type">Datetime</span>
    <span class="param-desc">Data dodania kroku</span>
  </li>
  <li>
    <span class="param-name">stage_id</span>
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
    <span class="param-name">status_id</span>
    <span class="param-type">int</span>
    <span class="param-desc">Status etapu:</span>
<ul class="status-values">
<li><code>1</code> — InProgress</li>
<li><code>2</code> — Done</li>
<li><code>3</code> — Error</li>
</ul>
  </li>
  <li>
    <span class="param-name">message</span>
    <span class="param-type">string</span>
    <span class="param-desc">Komunikat powiązany z etapem i statusem, np. na etapie Validation ze statusem Error: <code>"Import zawiera błędy walidacji"</code></span>
  </li>
  <li>
    <span class="param-name">stepDetailsList</span>
    <span class="param-type">StepDetail[]</span>
    <span class="param-desc">Lista szczegółowych statusów danego kroku (np. na kroku walidacji: lista błędów walidacji)</span>
  </li>
</ul>

**Struktura `StepDetail`:**

<ul class="param-list">
  <li>
    <span class="param-name">id</span>
    <span class="param-type">Guid</span>
    <span class="param-desc">ID szczegółowego statusu</span>
  </li>
  <li>
    <span class="param-name">description</span>
    <span class="param-type">string</span>
    <span class="param-desc">Tekstowy opis, np. <code>"W komunikacie ObjectID a0f22474-...: brak uzupełnionego pola Waluta"</code></span>
  </li>
  <li>
    <span class="param-name">import_step_details_type_id</span>
    <span class="param-type">int</span>
    <span class="param-desc">Typ szczegółowego statusu:</span>
<ul class="status-values">
<li><code>1</code> — Info</li>
<li><code>2</code> — Warning</li>
<li><code>3</code> — Error</li>
<li><code>4</code> — Ommit</li>
</ul>
  </li>
  <li>
    <span class="param-name">matchKey</span>
    <span class="param-type">string</span>
    <span class="param-desc">Wartość, której dotyczy naruszenie reguły walidacyjnej (np. wartość niepoprawnego pola albo identyfikator obiektu z danych źródłowych). <code>null</code> dla szczegółów niepochodzących z reguł walidacyjnych</span>
  </li>
  <li>
    <span class="param-name">matchKeyType</span>
    <span class="param-type">int</span>
    <span class="param-desc">Typ klucza w <code>matchKey</code> wg słownika <code>[dm_config].[match_key_type]</code> (ten sam słownik co w polach komunikatów, patrz <a href="../../../komunikaty/">Komunikaty</a>). <code>null</code>, gdy wartość nie jest kluczem ze słownika</span>
  </li>
  <li>
    <span class="param-name">brokenRule</span>
    <span class="param-type">int</span>
    <span class="param-desc">ID naruszonej reguły walidacyjnej, ta sama wartość co <code>Rules[].Id</code> w <a href="../set-import-validations/">SetImportValidations</a> (słownik <code>dm_config.validations</code>). <code>null</code> dla szczegółów niepochodzących z reguł walidacyjnych</span>
  </li>
</ul>

!!! info "Naruszenia reguł walidacyjnych"
    Naruszenia reguł egzekwowanych przez API (domyślnych oraz zarejestrowanych przez <a href="../set-import-validations/">SetImportValidations</a>) raportowane są jako szczegóły kroku **Consumption** (<code>stage_id=4</code>), bo reguły sprawdzane są per komunikat bezpośrednio przed zapisem danych. Jeden wpis odpowiada jednemu naruszeniu; przy ponownych próbach przetworzenia odrzuconego komunikatu wpisy mogą się powtórzyć.

**Struktura `ImportMessage`:**

<ul class="param-list">
  <li>
    <span class="param-name">queue_name</span>
    <span class="param-type">string</span>
    <span class="param-desc">Nazwa kolejki, na której przetwarzany jest komunikat</span>
  </li>
  <li>
    <span class="param-name">added</span>
    <span class="param-type">Datetime</span>
    <span class="param-desc">Data dodania komunikatu do kolejki</span>
  </li>
  <li>
    <span class="param-name">to_do_at</span>
    <span class="param-type">Datetime</span>
    <span class="param-desc">Data, po której komunikat może zostać pobrany do przetwarzania</span>
  </li>
  <li>
    <span class="param-name">processed</span>
    <span class="param-type">Datetime</span>
    <span class="param-desc">Data zakończenia przetwarzania. Puste, jeśli <code>to_do_at</code> jest w przyszłości</span>
  </li>
  <li>
    <span class="param-name">status_id</span>
    <span class="param-type">int</span>
    <span class="param-desc">Status przetwarzania komunikatu:</span>
<ul class="status-values">
<li><code>0</code> — OFFLINE — komunikat pomijany w bieżącym cyklu (status historyczny — nieużywany w trybie API-2-API)</li>
<li><code>1</code> — QUEUED — oczekuje na upłynięcie <code>to_do_at</code></li>
<li><code>2</code> — SUCCESS — poprawnie przetworzony</li>
<li><code>3</code> — ERROR — błąd przetwarzania</li>
<li><code>4</code> — REJECTED — odrzucony, nie podjęto próby przetwarzania</li>
<li><code>5</code> — INPROGRESS — w trakcie przetwarzania</li>
</ul>
  </li>
  <li>
    <span class="param-name">status_message</span>
    <span class="param-type">string</span>
    <span class="param-desc">Dodatkowy opis statusu (np. dla ERROR: opis błędu)</span>
  </li>
  <li>
    <span class="param-name">retries</span>
    <span class="param-type">int</span>
    <span class="param-desc">Aktualna próba przetwarzania komunikatu</span>
  </li>
  <li>
    <span class="param-name">object_id</span>
    <span class="param-type">string</span>
    <span class="param-desc">Identyfikator obiektu (patrz <a href="../../../komunikaty/">Komunikaty</a>)</span>
  </li>
  <li>
    <span class="param-name">version_id</span>
    <span class="param-type">long</span>
    <span class="param-desc">ID wersji obiektu w DM. <code>null</code>/<code>0</code>, gdy nie dotyczy</span>
  </li>
  <li>
    <span class="param-name">max_retry_count</span>
    <span class="param-type">int</span>
    <span class="param-desc">Maksymalna liczba prób. Jeśli <code>retries = max_retry_count</code>, komunikat nie będzie ponownie przetwarzany</span>
  </li>
</ul>

```json title="Przykład odpowiedzi"
{
  "importId": "2fa859e9-8479-4c7e-b1bb-c85f90f2402c",
  "importTypeName": "DM Integration API Sample - External Payments Import",
  "debtManagerImportId": 3,
  "debtManagerCreditorId": 10013,
  "debtManagerPortfolioId": 94,
  "isFinished": false,
  "currentStep": {
    "id": 42,
    "added": "2024-04-19T11:28:26.983",
    "stage_id": 4,
    "status_id": 1,
    "message": null,
    "stepDetailsList": []
  },
  "stepsHistory": [
    {
      "id": 36,
      "added": "2024-04-19T11:24:58.467",
      "stage_id": 1,
      "status_id": 1,
      "message": null,
      "stepDetailsList": []
    }
  ],
  "importMessages": [
    {
      "queue_name": "Case",
      "added": "2024-04-19T11:28:26.91",
      "to_do_at": "2024-04-19T11:28:26.87",
      "processed": null,
      "status_id": 5,
      "status_message": null,
      "retries": 0,
      "object_id": "bd459997-d8b3-4ae5-b00f-fe1a5042bd5a",
      "version_id": 21,
      "max_retry_count": 3
    }
  ],
  "externalFileName": null
}
```

!!! note "Nazewnictwo pól odpowiedzi"
    Odpowiedź jest serializowana z polityką **camelCase**, dlatego pola obiektu importu mają małą pierwszą literę (`importId`, `importTypeName`, `currentStep`...). Pola zagnieżdżonych struktur `ImportStep`, `StepDetail` i `ImportMessage` zachowują nazwy z bazy w formacie **snake_case** (`stage_id`, `status_id`, `queue_name`, `to_do_at`, `object_id`, `version_id`, `max_retry_count`, `import_step_details_type_id`). Używaj dokładnie tych kluczy.

!!! note "Skrócony przykład"
    Powyższy JSON zawiera skróconą historię kroków i komunikatów. W rzeczywistej odpowiedzi listy `stepsHistory` i `importMessages` mogą zawierać wiele elementów.

</div>
