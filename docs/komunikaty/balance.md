---
title: "Balance"
---

# Balance

<div class="msg-header">
  <span class="msg-type">Balance</span>
  <span class="msg-badge queue">kolejka: Balance</span>
  <span class="msg-badge db">dm_messages.balance_details</span>
  <p class="msg-desc">Komunikat pozwalający na zmianę salda na wybranym koncie dokumentu poprzez nadpisanie nową wartością. Wykorzystywane tylko przy importach, w których saldo przychodzi z systemu zewnętrznego i DM nie rozksięgowuje wpłat.</p>
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
<div class="api-section-title">Pola obiektu Balance</div>

<details class="collapsible-fields">
<summary>Balance — pola główne</summary>
<ul class="param-list">
  <li>
    <span class="param-name required">ObjectId</span>
    <span class="param-type">string</span>
    <span class="param-desc">Identyfikator obiektu do śledzenia statusu przetwarzania.</span>
  </li>
  <li>
    <span class="param-name required">AccountTypeId</span>
    <span class="param-type">int</span>
    <span class="param-desc">Typ konta księgowego (słownik).</span>
  </li>
  <li>
    <span class="param-name required">Amount</span>
    <span class="param-type">decimal</span>
    <span class="param-desc">Nowa kwota salda.</span>
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
    <span class="param-name">DueDate</span>
    <span class="param-type">datetime</span>
    <span class="param-desc">Data wymagalności.</span>
  </li>
  <li>
    <span class="param-name">InterestAccrualDate</span>
    <span class="param-type">datetime</span>
    <span class="param-desc">Data naliczenia odsetek.</span>
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
      "queueName": "Balance",
      "message": "{...}" // (1)
    }
    ```

    1. Pole `message` to **string zawierający JSON** — poniżej rozpakowana zawartość.

    ```json title="Zawartość pola message"
    {
      "Objects": [
        {
          "AccountTypeId": 2,
          "Amount": 44.44,
          "OperationDate": "2023-10-07T14:32:28.1234567",
          "BookingDate": "2023-10-07T14:32:28.1234567",
          "DueDate": "2023-10-07T14:32:28.1234567",
          "InterestAccrualDate": "2023-10-07T14:32:28.1234567",
          "MatchKey": "Case_MK_1",
          "MatchKeyType": 6,
          "ObjectId": "00000000-0000-0000-0000-000000000000",
          "ToDoAt": "2024-04-04T14:17:50.9982539+02:00"
        },
        {
          "AccountTypeId": 5,
          "Amount": 5.33,
          "OperationDate": "2023-10-07T14:32:28.1234567",
          "BookingDate": "2023-10-07T14:32:28.1234567",
          "DueDate": "2023-10-07T14:32:28.1234567",
          "InterestAccrualDate": "2023-10-07T14:32:28.1234567",
          "MatchKey": "Case_MK_1",
          "MatchKeyType": 6,
          "ObjectId": "00000000-0000-0000-0000-000000000001",
          "ToDoAt": "2024-04-04T14:17:50.9982539+02:00"
        }
      ],
      "ObjectsUpdateBehaviour": 6
    }
    ```

=== "Minimalny przykład"

    Nadpisanie salda na jednym koncie.

    ```json title="Koperta API"
    {
      "importId": "00000000-0000-0000-0000-000000000000",
      "queueName": "Balance",
      "message": "{...}"
    }
    ```

    ```json title="Zawartość pola message"
    {
      "Objects": [
        {
          "AccountTypeId": 2,
          "Amount": 44.44,
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
- Powiązane komunikaty: [Contract](contract.md) (wierzytelność), [ContractBalance](contract-balance.md) (zmiana salda wierzytelności), [Adjustment](adjustment.md) (korekty)

</div>
