---
title: "Adjustment"
---

# Adjustment

<div class="msg-header">
  <span class="msg-type">Adjustment</span>
  <span class="msg-badge queue">kolejka: Adjustment</span>
  <span class="msg-badge db">dm_messages.adjustment_details</span>
  <p class="msg-desc">Komunikat dodaje korekty.</p>
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
        <span class="mk-id">3</span>
        <span class="mk-label">Zewnętrzny numer umowy</span>
        <span class="mk-field">→ wierzytelnosc.wi_ext_id</span>
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
    </ul>
  </li>
</ul>

</div>

---

<div class="api-section">
<div class="api-section-title">Pola obiektu Adjustment</div>

<details class="collapsible-fields">
<summary>Adjustment — pola główne</summary>
<ul class="param-list">
  <li>
    <span class="param-name required">ObjectId</span>
    <span class="param-type">string</span>
    <span class="param-desc">Identyfikator obiektu do śledzenia statusu przetwarzania.</span>
  </li>
  <li>
    <span class="param-name required">AccountId</span>
    <span class="param-type">int</span>
    <span class="param-desc">ID konta księgowego.</span>
  </li>
  <li>
    <span class="param-name required">Amount</span>
    <span class="param-type">decimal</span>
    <span class="param-desc">Kwota korekty.</span>
  </li>
  <li>
    <span class="param-name">Description</span>
    <span class="param-type">string</span>
    <span class="param-desc">Opis korekty.</span>
  </li>
  <li>
    <span class="param-name">CurrencyId</span>
    <span class="param-type">int</span>
    <span class="param-desc">ID waluty (słownik).</span>
  </li>
  <li>
    <span class="param-name">ExchangeRate</span>
    <span class="param-type">decimal</span>
    <span class="param-desc">Kurs wymiany waluty.</span>
  </li>
  <li>
    <span class="param-name">OperationDate</span>
    <span class="param-type">datetime</span>
    <span class="param-desc">Data operacji.</span>
  </li>
  <li>
    <span class="param-name">BookingDate</span>
    <span class="param-type">datetime</span>
    <span class="param-desc">Data księgowania.</span>
  </li>
  <li>
    <span class="param-name">ToDoAt</span>
    <span class="param-type">datetime</span>
    <span class="param-desc">Data zaplanowanego wykonania.</span>
  </li>
  <li>
    <span class="param-name">SnapshotDate</span>
    <span class="param-type">object</span>
    <span class="param-desc">Data migawki — obiekt z polem <code>Date</code> (datetime).</span>
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
      "queueName": "Adjustment",
      "message": "{...}" // (1)
    }
    ```

    1. Pole `message` to **string zawierający JSON** — poniżej rozpakowana zawartość.

    ```json title="Zawartość pola message"
    {
      "Objects": [
        {
          "AccountId": 2,
          "Amount": 20,
          "Description": "Test",
          "CurrencyId": 1,
          "ExchangeRate": 1,
          "OperationDate": "2023-10-07T14:32:28.1234567",
          "BookingDate": "2023-10-07T14:32:28.1234567",
          "ToDoAt": "2024-04-04T14:17:50.9982539+02:00",
          "SnapshotDate": {
            "Date": "2025-02-15T00:00:00"
          },
          "MatchKey": "WRO00/W/WTSPR/9875841/749/23_10013_94",
          "MatchKeyType": 6,
          "ObjectId": "00000000-0000-0000-0000-000000000000"
        },
        {
          "AccountId": 5,
          "Amount": 5,
          "Description": "Test",
          "CurrencyId": 1,
          "ExchangeRate": 1,
          "SnapshotDate": {
            "Date": "2025-02-15T00:00:00"
          },
          "MatchKey": "WRO00/W/WTSPR/9875841/749/23_10013_94",
          "MatchKeyType": 6,
          "ObjectId": "00000000-0000-0000-0000-000000000001",
          "ToDoAt": "2024-04-04T14:17:50.9982539+02:00"
        }
      ],
      "ObjectsUpdateBehaviour": 6
    }
    ```

=== "Minimalny przykład"

    Dodanie pojedynczej korekty na sprawie.

    ```json title="Koperta API"
    {
      "importId": "00000000-0000-0000-0000-000000000000",
      "queueName": "Adjustment",
      "message": "{...}"
    }
    ```

    ```json title="Zawartość pola message"
    {
      "Objects": [
        {
          "AccountId": 2,
          "Amount": 20,
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
- Powiązane komunikaty: [Case](case.md) (sprawa), [Contract](contract.md) (wierzytelność), [Payment](payment.md) (płatności), [Balance](balance.md) (nadpisanie salda)

</div>
