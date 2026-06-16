---
title: "SetImportValidations"
---

# SetImportValidations

<div class="endpoint-header">
  <div class="method-badge post">POST</div>
  <div class="endpoint-url">https://dmapi-intrum-dev.groupad1.com/pl/IntegrationsAPI/import/SetImportValidations</div>
</div>

Rejestruje dla importu reguły walidacyjne egzekwowane przez API. Reguły są sprawdzane w trakcie konsumpcji (Stage 4), per komunikat, przed zapisem danych do DEBT Manager. Naruszenia reguł są raportowane w szczegółach kroku Consumption w [GetImportStatus](get-import-status.md) (pola `MatchKey`, `MatchKeyType`, `BrokenRule` w strukturze `StepDetail`).

!!! info "Model działania reguł"
    -   **Reguły domyślne** (krytyczne reguły referencyjne i techniczne, np. poprawność powiązań między obiektami) działają **zawsze**, niezależnie od rejestracji. Nie da się ich wyłączyć ani zmienić ich poziomu: wpis w `Rules` dla takiej reguły jest ignorowany.
    -   **Pozostałe reguły** działają tylko wtedy, gdy zostaną zarejestrowane dla danego importu przez to wywołanie. Reguła niezarejestrowana nie jest sprawdzana.
    -   Import bez wywołania `SetImportValidations` jest walidowany wyłącznie regułami domyślnymi.

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
    <span class="param-desc">ID reguły walidacyjnej ze słownika <code>dm_config.validations</code>. Ta sama wartość wraca w polu <code>BrokenRule</code> szczegółów kroku w <a href="../get-import-status/">GetImportStatus</a></span>
  </li>
  <li>
    <span class="param-name required">Level</span>
    <span class="param-type">int</span>
    <span class="param-desc">Zachowanie reguły dla tego importu:</span>
<ul class="status-values">
<li><code>1</code> — Info — naruszenia są tylko raportowane (szczegół kroku typu Info); komunikaty przetwarzane normalnie</li>
<li><code>2</code> — Warning — naruszenia są tylko raportowane (szczegół kroku typu Warning); komunikaty przetwarzane normalnie</li>
<li><code>3</code> — Error — komunikat naruszający regułę jest odrzucany (status komunikatu <code>ERROR</code>, dane nie zapisują się do DM); pozostałe komunikaty przetwarzane normalnie; naruszenia raportowane jako szczegół kroku typu Error. Import z odrzuconymi komunikatami zakończy się statusem Error</li>
<li><code>4</code> — Ommit — reguła jest sprawdzana, ale nie blokuje: naruszenia są raportowane jako szczegół kroku typu Ommit, a komunikaty przetwarzane normalnie</li>
</ul>
  </li>
</ul>

```json title="Przykład request body"
{
  "ImportId": "10000000-0000-0000-0000-000000000000",
  "Rules": [
    {
      "Id": 3001,
      "Level": 3
    },
    {
      "Id": 4002,
      "Level": 2
    }
  ]
}
```

</div>

---

<div class="api-section" markdown>
<div class="api-section-title">Response</div>

- **Sukces:** HTTP `200 OK`, pusty string w treści.
- **Błąd:** HTTP `400 Bad Request` z tekstowym opisem błędu (np. nieprawidłowe `Id` reguły). Błąd nie jest już zwracany jako `200 OK`.

</div>
