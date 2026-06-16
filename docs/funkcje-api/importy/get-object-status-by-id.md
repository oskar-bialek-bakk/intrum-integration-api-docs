---
title: "GetObjectStatusByID"
---

# GetObjectStatusByID

<div class="endpoint-header">
  <div class="method-badge get">GET</div>
  <div class="endpoint-url">https://dmapi-intrum-dev.groupad1.com/pl/IntegrationsAPI/import/GetObjectStatusByID</div>
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
curl "https://dmapi-intrum-dev.groupad1.com/pl/IntegrationsAPI/import/GetObjectStatusByID?queueName=Customer&objectId=f40430d2-5c1e-4dff-8caf-f73af2f0301f" \
  -H "Authorization: Bearer {TOKEN}"
```

</div>

---

<div class="api-section" markdown>
<div class="api-section-title">Response</div>

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
<li><code>4</code> — REJECTED — odrzucony</li>
<li><code>5</code> — INPROGRESS — w trakcie przetwarzania</li>
</ul>
  </li>
  <li>
    <span class="param-name">status_message</span>
    <span class="param-type">string</span>
    <span class="param-desc">Dodatkowy opis statusu (np. dla ERROR: opis błędu). Komunikat odrzucony przez regułę walidacyjną o poziomie Error ma tu informację o liczbie naruszeń, np. <code>"Validation failed: 2 BLOCKING violation(s)"</code>; szczegóły naruszeń (pola, wartości, ID reguł) są w szczegółach kroku Consumption w <a href="../get-import-status/">GetImportStatus</a></span>
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

!!! note "Nazewnictwo pól odpowiedzi"
    Pola struktury `ImportMessage` zachowują nazwy z bazy w formacie **snake_case** (`queue_name`, `to_do_at`, `status_id`, `status_message`, `object_id`, `version_id`, `max_retry_count`). Używaj dokładnie tych kluczy.

```json title="Przykład odpowiedzi"
{
  "queue_name": "Customer",
  "added": "2024-04-29T15:38:24.36",
  "to_do_at": "2024-04-29T15:38:24.29",
  "processed": "2024-04-29T15:38:30.663",
  "status_id": 2,
  "status_message": "",
  "retries": 0,
  "object_id": "f40430d2-5c1e-4dff-8caf-f73af2f0301f",
  "version_id": 18,
  "max_retry_count": 3
}
```

</div>
