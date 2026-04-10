---
title: "SetImportValidations"
---

# SetImportValidations

<div class="endpoint-header">
  <div class="method-badge post">POST</div>
  <div class="endpoint-url">https://[adres_api]/SetImportValidations</div>
</div>

Uruchamia walidacje sprawdzane w ramach kroku walidacyjnego w imporcie. Pozwala zdefiniować reguły walidacyjne i ich poziom ważności.

---

<div class="api-section" markdown>
<div class="api-section-title">Request body</div>

<ul class="param-list">
  <li>
    <span class="param-name required">ImportId</span>
    <span class="param-type">Guid</span>
    <span class="param-desc">ID importu</span>
  </li>
  <li>
    <span class="param-name required">Rules</span>
    <span class="param-type">ValidationRule[]</span>
    <span class="param-desc">Lista reguł walidacyjnych</span>
  </li>
</ul>

**Struktura `ValidationRule`:**

<ul class="param-list">
  <li>
    <span class="param-name required">Id</span>
    <span class="param-type">int</span>
    <span class="param-desc">ID warunku walidacyjnego z tabeli <code>dm_config.validations</code></span>
  </li>
  <li>
    <span class="param-name required">Level</span>
    <span class="param-type">int</span>
    <span class="param-desc">Zachowanie walidacyjne:</span>
<ul class="status-values">
<li><code>1</code> — Info — komunikat informacyjny</li>
<li><code>2</code> — Warning — ostrzeżenie przed potencjalnymi problemami</li>
<li><code>3</code> — Error — błąd walidacyjny (jeden błąd powoduje odrzucenie całego importu)</li>
<li><code>4</code> — Ommit — komunikat spełniający tę regułę jest pomijany w imporcie (pozostałe wczytywane normalnie)</li>
</ul>
  </li>
</ul>

```json title="Przykład request body"
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

</div>

---

<div class="api-section" markdown>
<div class="api-section-title">Response</div>

Pusty string w przypadku sukcesu. Tekstowy opis błędu w przypadku błędu.

</div>
