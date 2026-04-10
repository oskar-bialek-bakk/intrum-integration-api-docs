---
title: "AttributeCase"
---

# AttributeCase

<div class="msg-header">
  <span class="msg-type">AttributeCase</span>
  <span class="msg-badge queue">kolejka: AttributeCase</span>
  <span class="msg-badge db">dm_attributeCase_details</span>
  <p class="msg-desc">Komunikat dodaje/aktualizuje atrybuty sprawy. Dla jednej sprawy może być tylko jeden atrybut danego typu.</p>
</div>

---

<div class="api-section" markdown>
<div class="api-section-title">Pola wspólne (MatchKey)</div>

<ul class="param-list">
  <li>
    <span class="param-name required">MatchKey</span>
    <span class="param-type">string</span>
    <span class="param-desc">Klucz wyszukiwania obiektu w DM. Patrz <a href="index.md#budowa-komunikatów">Budowa komunikatów</a>.</span>
  </li>
  <li>
    <span class="param-name required">MatchKeyType</span>
    <span class="param-type">int</span>
    <span class="param-desc">Typ klucza wyszukiwania. Dopuszczalne wartości:</span>
    <ul class="mk-values">
      <li>
        <span class="mk-id">6</span>
        <span class="mk-label">Zewnętrzny numer sprawy</span>
        <span class="mk-field">→ sprawa.sp_ext_id</span>
      </li>
    </ul>
  </li>
</ul>

</div>

---

<div class="api-section" markdown>
<div class="api-section-title">Pola obiektu AttributeCase</div>

!!! info "Uwaga"
    Ten komunikat używa formatu tablicy (`[...]`) zamiast wrappera `{ "Objects": [...] }`.

<details class="collapsible-fields">
<summary>AttributeCase — pola główne</summary>
<ul class="param-list">
  <li>
    <span class="param-name required">ObjectId</span>
    <span class="param-type">string</span>
    <span class="param-desc">Identyfikator obiektu do śledzenia statusu przetwarzania.</span>
  </li>
  <li>
    <span class="param-name required">TypeId</span>
    <span class="param-type">int</span>
    <span class="param-desc">ID typu atrybutu ze słownika <code>atrybut_typ</code>.</span>
  </li>
  <li>
    <span class="param-name required">Value</span>
    <span class="param-type">string</span>
    <span class="param-desc">Wartość atrybutu.</span>
  </li>
  <li>
    <span class="param-name">ToDoAt</span>
    <span class="param-type">datetime</span>
    <span class="param-desc">Data zaplanowanego wykonania.</span>
  </li>
</ul>
</details>

</div>

---

<div class="api-section" markdown>
<div class="api-section-title">Przykład JSON</div>

=== "Pełny komunikat"

    ```json
    {
      "importId": "00000000-0000-0000-0000-000000000000",
      "queueName": "AttributeCase",
      "message": "{...}" // (1)
    }
    ```

    1. Pole `message` to **string zawierający JSON** — poniżej rozpakowana zawartość.

    ```json title="Zawartość pola message"
    [
      {
        "MatchKey": "SZC00/W/WTSPR/9877817/33/24_10013_94",
        "MatchKeyType": 6,
        "ObjectId": "00000000-0000-0000-0000-000000000000",
        "ToDoAt": "2024-04-08T08:59:50.9982539+02:00",
        "TypeId": 7682,
        "Value": "15462"
      }
    ]
    ```

=== "Minimalny przykład"

    ```json title="Koperta API"
    {
      "importId": "00000000-0000-0000-0000-000000000000",
      "queueName": "AttributeCase",
      "message": "{...}"
    }
    ```

    ```json title="Zawartość pola message"
    [
      {
        "MatchKey": "SP/2024/001",
        "MatchKeyType": 6,
        "ObjectId": "00000000-0000-0000-0000-000000000000",
        "TypeId": 7682,
        "Value": "15462"
      }
    ]
    ```

</div>

---

<div class="api-section" markdown>
<div class="api-section-title">Powiązania</div>

- Wysyłanie komunikatu — endpoint [EnqueImportMessage](../funkcje-api/importy/enque-import-message.md)
- Powiązane komunikaty: [Case](case.md) (sprawa), [AttributeDebtor](attribute-debtor.md) (atrybuty dłużnika)

</div>
