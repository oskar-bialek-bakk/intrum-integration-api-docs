---
title: "SetImportStep"
---

# SetImportStep

<div class="endpoint-header">
  <div class="method-badge post">POST</div>
  <div class="endpoint-url">https://[adres_api]/SetImportStep</div>
</div>

Ustawia aktualny krok importu — pozwala zgłosić zmianę etapu lub statusu w procesie importu.

---

<div class="api-section" markdown>
<div class="api-section-title">Request</div>

**Query parameters:**

<ul class="param-list">
  <li>
    <span class="param-name required">importId</span>
    <span class="param-type">Guid</span>
    <span class="param-desc">ID importu</span>
  </li>
</ul>

**Body** (JSON) — obiekt `ImportStep`:

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
    <span class="param-name required">Stage</span>
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
    <span class="param-name required">StageStatus</span>
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
    <span class="param-desc">Komunikat powiązany z etapem i statusem, np. <code>"Plik importowy zawiera błędy walidacji"</code></span>
  </li>
  <li>
    <span class="param-name">StepDetailsList</span>
    <span class="param-type">StepDetail[]</span>
    <span class="param-desc">Lista szczegółowych statusów danego kroku</span>
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
</ul>
  </li>
  <li>
    <span class="param-name">MatchKey</span>
    <span class="param-type">string</span>
    <span class="param-desc">Opcjonalne — pozwala powiązać szczegół z konkretną encją w bazie danych (w kombinacji z <code>MatchKeyType</code>)</span>
  </li>
  <li>
    <span class="param-name">MatchKeyType</span>
    <span class="param-type">int</span>
    <span class="param-desc">Opcjonalne — typ encji, z którą powiązany jest <code>MatchKey</code></span>
  </li>
</ul>

```json title="Przykład request body"
{
  "Id": 0,
  "Added": "2024-04-19T12:00:00",
  "Stage": 3,
  "StageStatus": 2,
  "Message": null,
  "StepDetailsList": []
}
```

</div>

---

<div class="api-section" markdown>
<div class="api-section-title">Response</div>

Struktura odpowiedzi jest identyczna jak w funkcji [GetImportStatus](get-import-status.md).

</div>

---

<div class="api-section" markdown>
<div class="api-section-title">Możliwe zmiany statusów</div>

![Etapy importu](../../diagrams/import-stages-state.drawio)

**Stage** — etapy importu:

<div class="pipeline">
  <div class="pipeline-step">
    <span class="step-num">1</span>
    <span class="step-title">Adding</span>
    <span class="step-desc">Dodawanie danych importowych do systemu.</span>
  </div>
  <span class="pipeline-arrow">&#x2192;</span>
  <div class="pipeline-step">
    <span class="step-num">2</span>
    <span class="step-title">Transformation</span>
    <span class="step-desc">Transformacja danych na komunikaty zrozumiałe przez API.</span>
  </div>
  <span class="pipeline-arrow">&#x2192;</span>
  <div class="pipeline-step">
    <span class="step-num">3</span>
    <span class="step-title">Validation</span>
    <span class="step-desc">Walidacja danych do importu, odrzucenie w przypadku błędów.</span>
  </div>
  <span class="pipeline-arrow">&#x2192;</span>
  <div class="pipeline-step">
    <span class="step-num">4</span>
    <span class="step-title">Consumption</span>
    <span class="step-desc">Konsumpcja komunikatów z kolejek i zasilenie systemu DM.</span>
  </div>
  <span class="pipeline-arrow">&#x2192;</span>
  <div class="pipeline-step">
    <span class="step-num">5</span>
    <span class="step-title">Finishing</span>
    <span class="step-desc">Finalizacja importu i raportowanie wyników.</span>
  </div>
</div>

**StageStatus** — status etapu:

<ul class="status-values">
<li><code>1</code> — InProgress</li>
<li><code>2</code> — Done</li>
<li><code>3</code> — Error</li>
</ul>

</div>
