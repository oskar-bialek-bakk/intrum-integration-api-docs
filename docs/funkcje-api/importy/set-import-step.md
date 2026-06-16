---
title: "SetImportStep"
---

# SetImportStep

<div class="endpoint-header">
  <div class="method-badge post">POST</div>
  <div class="endpoint-url">https://dmapi-intrum-dev.groupad1.com/pl/IntegrationsAPI/import/SetImportStep</div>
</div>

Pozwala Klientowi API sterować cyklem życia importu — zgłosić rozpoczęcie fazy Adding po wywołaniu [CreateImport](create-import.md), zamknąć fazę Adding po wysłaniu komunikatów, opcjonalnie przejąć etap Validation (gdy Klient API waliduje dane u siebie), oraz raportować błędy z poziomu klienta.

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
    <span class="param-name required">Stage</span>
    <span class="param-type">int</span>
    <span class="param-desc">Etap importu (wartości, które ustawia Klient API):</span>
<ul class="status-values">
<li><code>1</code> — Adding</li>
<li><code>3</code> — Validation</li>
<li><code>5</code> — Finishing</li>
</ul>
<span class="param-desc">Etap <code>2</code> (Transformation) i <code>4</code> (Consumption) ustawia wyłącznie API wewnętrznie. W trybie API-2-API faza Transformation jest pomijana.</span>
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
</ul>

**Pola zaawansowane (opcjonalne w podstawowym użyciu):**

<ul class="param-list">
  <li>
    <span class="param-name">Id</span>
    <span class="param-type">int</span>
    <span class="param-desc">Zwykle pomijany przy zgłaszaniu zmiany statusu; identyfikuje konkretny krok przy korektach.</span>
  </li>
  <li>
    <span class="param-name">Added</span>
    <span class="param-type">Datetime</span>
    <span class="param-desc">Moment kroku; jeśli pominięty, API przyjmuje czas bieżący (UTC). Zwykle pomijany.</span>
  </li>
  <li>
    <span class="param-name">Message</span>
    <span class="param-type">string</span>
    <span class="param-desc">Opcjonalny komentarz powiązany z etapem/statusem (np. powód błędu przy <code>StageStatus=3</code>).</span>
  </li>
  <li>
    <span class="param-name">StepDetailsList</span>
    <span class="param-type">StepDetail[]</span>
    <span class="param-desc">Szczegółowe wpisy (np. lista naruszeń), używane gdy klient sam raportuje walidację. Zwykle pusta/pominięta.</span>
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
    <span class="param-desc">Tekstowy opis, np. <code>"W komunikacie ObjectID a0f22474-...: brak uzupełnionego pola Waluta"</code></span>
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

To typowy przypadek: Klient API zgłasza, że etap Validation został ukończony po jego stronie (importId podaje w query).

```json title="Przykład minimalny (oznaczenie zmiany statusu)"
{
  "Stage": 3,
  "StageStatus": 2
}
```

Pola zaawansowane wypełnia się tylko, gdy są potrzebne (np. `Message` z opisem, `StepDetailsList` przy własnej walidacji).

```json title="Przykład pełny (z polami zaawansowanymi)"
{
  "Stage": 3,
  "StageStatus": 2,
  "Message": "Walidacja po stronie klienta zakończona",
  "StepDetailsList": []
}
```

!!! note "Dozwolone przejścia Klienta API"
    API egzekwuje maszynę stanów importu. Klient API może ustawić wyłącznie następujące pary `Stage`/`StageStatus`:

    - `1`/`1` — Adding / InProgress (start fazy dodawania, zaraz po `CreateImport`),
    - `1`/`2` — Adding / Done (zamknięcie dodawania; API samo uruchamia walidację po swojej stronie),
    - `3`/`2` — Validation / Done (gdy Klient API waliduje dane u siebie i pomija walidację API),
    - `5`/`2` lub `5`/`3` — Finishing / Done albo Finishing / Error,
    - `Error` (StageStatus `3`) na bieżącym etapie.

    Inne wartości (np. `Stage=2` Transformation) zwrócą błąd `Forbidden import state change`.

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
    <span class="step-desc">Transformacja danych po stronie API. Faza wewnętrzna, pomijana w trybie API-2-API.</span>
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
