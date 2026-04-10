---
title: "Signature"
---

# Signature

<div class="msg-header">
  <span class="msg-type">Signature</span>
  <span class="msg-badge queue">kolejka: Signature</span>
  <span class="msg-badge db">dm_messages.signature_details</span>
  <p class="msg-desc">Komunikat dodaje sygnatury.</p>
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

<div class="api-section">
<div class="api-section-title">Pola obiektu Signature</div>

<details class="collapsible-fields">
<summary>Signature — pola główne</summary>
<ul class="param-list">
  <li>
    <span class="param-name required">ObjectId</span>
    <span class="param-type">string</span>
    <span class="param-desc">Identyfikator obiektu do śledzenia statusu przetwarzania.</span>
  </li>
  <li>
    <span class="param-name required">SignatureTypeId</span>
    <span class="param-type">int</span>
    <span class="param-desc">Typ sygnatury (słownik).</span>
  </li>
  <li>
    <span class="param-name required">SignatureNumber</span>
    <span class="param-type">string</span>
    <span class="param-desc">Numer sygnatury (np. <code>Nc-e xxxxxxx/xx</code>).</span>
  </li>
  <li>
    <span class="param-name">SignatureDateFrom</span>
    <span class="param-type">datetime</span>
    <span class="param-desc">Data rozpoczęcia obowiązywania sygnatury.</span>
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
      "queueName": "Signature",
      "message": "{...}" // (1)
    }
    ```

    1. Pole `message` to **string zawierający JSON** — poniżej rozpakowana zawartość.

    ```json title="Zawartość pola message"
    {
      "Objects": [
        {
          "SignatureTypeId": 2,
          "SignatureNumber": "Nc-e xxxxxxx/xx",
          "SignatureDateFrom": "2024-04-04T12:20:50.7489216+02:00",
          "ToDoAt": "2024-04-04T14:17:50.9982539+02:00",
          "MatchKey": "Case_MK_1",
          "MatchKeyType": 6,
          "ObjectId": "00000000-0000-0000-0000-000000000000"
        }
      ],
      "ObjectsUpdateBehaviour": 6
    }
    ```

=== "Minimalny przykład"

    ```json title="Koperta API"
    {
      "importId": "00000000-0000-0000-0000-000000000000",
      "queueName": "Signature",
      "message": "{...}"
    }
    ```

    ```json title="Zawartość pola message"
    {
      "Objects": [
        {
          "SignatureTypeId": 2,
          "SignatureNumber": "Nc-e 1234567/24",
          "MatchKey": "Case_MK_1",
          "MatchKeyType": 6,
          "ObjectId": "00000000-0000-0000-0000-000000000000"
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
- Powiązane komunikaty: [Case](case.md) (sprawa), [Customer](customer.md) (dłużnik)

</div>
