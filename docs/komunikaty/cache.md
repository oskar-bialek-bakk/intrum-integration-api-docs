---
title: "Cache"
---

# Cache

<div class="msg-header">
  <span class="msg-type">Cache</span>
  <span class="msg-badge queue">kolejka: cache</span>
  <span class="msg-badge db">dm_messages.cache_details</span>
  <p class="msg-desc">Komunikat odświeża wybrane domeny cache dla danego match key. Jest to komunikat techniczny — może pojawić się w bazie w wyniku przetwarzania innych komunikatów lub ręcznego zakolejkowania w EnqueueMessage.</p>
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
        <span class="mk-id">1</span>
        <span class="mk-label">Numer rachunku bankowego</span>
        <span class="mk-field">→ rachunek_bankowy.rb_nr</span>
      </li>
      <li>
        <span class="mk-id">2</span>
        <span class="mk-label">Zewnętrzny numer dłużnika</span>
        <span class="mk-field">→ dluznik.dl_ext_id</span>
      </li>
      <li>
        <span class="mk-id">3</span>
        <span class="mk-label">Zewnętrzny numer umowy</span>
        <span class="mk-field">→ wierzytelnosc.wi_ext_id</span>
      </li>
      <li>
        <span class="mk-id">4</span>
        <span class="mk-label">Numer sprawy</span>
        <span class="mk-field">→ sprawa.sp_numer</span>
      </li>
      <li>
        <span class="mk-id">5</span>
        <span class="mk-label">Zewnętrzny numer dokumentu</span>
        <span class="mk-field">→ dokument.do_ext_id</span>
      </li>
      <li>
        <span class="mk-id">6</span>
        <span class="mk-label">Zewnętrzny numer sprawy</span>
        <span class="mk-field">→ sprawa.sp_ext_id</span>
      </li>
      <li>
        <span class="mk-id">20</span>
        <span class="mk-label">Wszystkie sprawy windykacyjne powiązane z zewnętrznym ID wierzytelności</span>
        <span class="mk-field">→ wierzytelnosc.wi_ext_id</span>
      </li>
      <li>
        <span class="mk-id">21</span>
        <span class="mk-label">Dowolna sprawa windykacyjna powiązana z zewnętrznym ID wierzytelności</span>
        <span class="mk-field">→ wierzytelnosc.wi_ext_id</span>
      </li>
    </ul>
  </li>
</ul>

</div>

---

<div class="api-section">
<div class="api-section-title">Pola obiektu Cache</div>

<details class="collapsible-fields">
<summary>Cache — pola główne</summary>
<ul class="param-list">
  <li>
    <span class="param-name required">ObjectId</span>
    <span class="param-type">string</span>
    <span class="param-desc">Identyfikator obiektu do śledzenia statusu przetwarzania.</span>
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
      "queueName": "cache",
      "message": "{...}" // (1)
    }
    ```

    1. Pole `message` to **string zawierający JSON** — poniżej rozpakowana zawartość.

    ```json title="Zawartość pola message"
    {
      "Objects": [
        {
          "MatchKey": "3_15237574252923798622544812_200785453",
          "MatchKeyType": 6,
          "ObjectId": "00000000-0000-0000-0000-000000000001",
          "ToDoAt": "2024-04-08T09:04:10.9982539+02:00"
        },
        {
          "MatchKey": "3_96164366624736830645821115_7743999",
          "MatchKeyType": 6,
          "ObjectId": "00000000-0000-0000-0000-000000000002",
          "ToDoAt": "2024-04-08T09:04:10.9982539+02:00"
        }
      ],
      "ObjectsUpdateBehaviour": 6
    }
    ```

=== "Minimalny przykład"

    ```json title="Koperta API"
    {
      "importId": "00000000-0000-0000-0000-000000000000",
      "queueName": "cache",
      "message": "{...}"
    }
    ```

    ```json title="Zawartość pola message"
    {
      "Objects": [
        {
          "MatchKey": "Case_MK_1",
          "MatchKeyType": 6,
          "ObjectId": "00000000-0000-0000-0000-000000000001"
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

</div>
