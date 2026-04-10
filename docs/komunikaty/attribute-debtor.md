---
title: "AttributeDebtor"
---

# AttributeDebtor

<div class="msg-header">
  <span class="msg-type">AttributeDebtor</span>
  <span class="msg-badge queue">kolejka: AttributeDebtor</span>
  <span class="msg-badge db">dm_attributeDebtor_details</span>
  <p class="msg-desc">Komunikat dodaje/aktualizuje atrybuty klienta. Dla klienta może być tylko jeden atrybut danego typu.</p>
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
        <span class="mk-id">2</span>
        <span class="mk-label">Zewnętrzny numer dłużnika</span>
        <span class="mk-field">→ dluznik.dl_ext_id</span>
      </li>
    </ul>
  </li>
</ul>

</div>

---

<div class="api-section">
<div class="api-section-title">Pola obiektu AttributeDebtor</div>

<details class="collapsible-fields">
<summary>AttributeDebtor — pola główne</summary>
<ul class="param-list">
  <li>
    <span class="param-name required">ObjectId</span>
    <span class="param-type">string</span>
    <span class="param-desc">Identyfikator obiektu do śledzenia statusu przetwarzania.</span>
  </li>
  <li>
    <span class="param-name required">TypeId</span>
    <span class="param-type">int</span>
    <span class="param-desc">ID typu atrybutu (słownik).</span>
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
      "queueName": "AttributeDebtor",
      "message": "{...}" // (1)
    }
    ```

    1. Pole `message` to **string zawierający JSON** — poniżej rozpakowana zawartość.

    ```json title="Zawartość pola message"
    {
      "Objects": [
        {
          "MatchKey": "Customer_MK_1",
          "MatchKeyType": 2,
          "ObjectId": "00000000-0000-0000-0000-000000000000",
          "ToDoAt": "2024-04-08T08:59:50.9982539+02:00",
          "TypeId": 2,
          "Value": "90"
        }
      ],
      "ObjectsUpdateBehaviour": 6
    }
    ```

=== "Minimalny przykład"

    ```json title="Koperta API"
    {
      "importId": "00000000-0000-0000-0000-000000000000",
      "queueName": "AttributeDebtor",
      "message": "{...}"
    }
    ```

    ```json title="Zawartość pola message"
    {
      "Objects": [
        {
          "MatchKey": "Customer_MK_1",
          "MatchKeyType": 2,
          "ObjectId": "00000000-0000-0000-0000-000000000000",
          "TypeId": 2,
          "Value": "90"
        }
      ],
      "ObjectsUpdateBehaviour": 6
    }
    ```

</div>

---

<div class="api-section" markdown>
<div class="api-section-title">Powiązania</div>

- Wysyłanie komunikatu — endpoint [EnqueImportMessage](../funkcje-api/importy/enque-import-message.md)
- Macierz flag aktualizacyjnych — [ObjectsUpdateBehaviour](index.md#flagi-aktualizacyjne-obiektow-objectsupdatebehaviour)
- Powiązane komunikaty: [Customer](customer.md) (dłużnik), [AttributeCase](attribute-case.md) (atrybuty sprawy)

</div>
