---
title: "GetObjectStatusByID"
---

# GetObjectStatusByID

<div class="endpoint-header">
  <div class="method-badge get">GET</div>
  <div class="endpoint-url">https://[adres_api]/GetObjectStatusByID</div>
</div>

Pobiera aktualny status danego obiektu (komunikatu) na wybranej kolejce.

---

<div class="api-section" markdown>
<div class="api-section-title">Request</div>

**Query parameters:**

<ul class="param-list">
  <li>
    <span class="param-name required">queueName</span>
    <span class="param-type">string</span>
    <span class="param-desc">Nazwa kolejki, o którą odpytujemy</span>
  </li>
  <li>
    <span class="param-name required">objectId</span>
    <span class="param-type">Guid</span>
    <span class="param-desc">ID obiektu, o którego status odpytujemy</span>
  </li>
</ul>

```bash title="Przykład wywołania"
curl "https://[adres_api]/GetObjectStatusByID?queueName=Customer&objectId=f40430d2-5c1e-4dff-8caf-f73af2f0301f" \
  -H "Authorization: Bearer {TOKEN}"
```

</div>

---

<div class="api-section" markdown>
<div class="api-section-title">Response</div>

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
<li><code>0</code> — OFFLINE — komunikat ignorowany (czeka na rozbicie pliku)</li>
<li><code>1</code> — QUEUED — oczekuje na upłynięcie <code>ToDoAt</code></li>
<li><code>2</code> — SUCCESS — poprawnie przetworzony</li>
<li><code>3</code> — ERROR — błąd przetwarzania</li>
<li><code>4</code> — REJECTED — odrzucony</li>
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

</div>
