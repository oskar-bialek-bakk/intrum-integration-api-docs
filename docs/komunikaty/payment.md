---
title: "Payment"
---

# Payment

<div class="msg-header">
  <span class="msg-type">Payment</span>
  <span class="msg-badge queue">kolejka: Payment</span>
  <span class="msg-badge db">dm_messages.payment_details</span>
  <p class="msg-desc">Komunikat dodaje płatności.</p>
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
        <span class="mk-id">4</span>
        <span class="mk-label">Numer sprawy</span>
        <span class="mk-field">→ sprawa.sp_numer</span>
      </li>
      <li>
        <span class="mk-id">6</span>
        <span class="mk-label">Zewnętrzny numer sprawy</span>
        <span class="mk-field">→ sprawa.sp_ext_id</span>
      </li>
      <li>
        <span class="mk-id">21</span>
        <span class="mk-label">Dowolna sprawa windykacyjna powiązana z zewnętrznym ID wierzytelności</span>
        <span class="mk-field">→ wierzytelnosc.wi_ext_id</span>
      </li>
      <li>
        <span class="mk-id">22</span>
        <span class="mk-label">Dowolna sprawa windykacyjna powiązana z zewnętrznym ID dokumentu</span>
        <span class="mk-field">→ dokument.do_ext_id</span>
      </li>
    </ul>
  </li>
</ul>

</div>

---

<div class="api-section">
<div class="api-section-title">Pola obiektu Payment</div>

<details class="collapsible-fields">
<summary>Payment — pola główne</summary>
<ul class="param-list">
  <li>
    <span class="param-name required">ObjectId</span>
    <span class="param-type">string</span>
    <span class="param-desc">Identyfikator obiektu do śledzenia statusu przetwarzania.</span>
  </li>
  <li>
    <span class="param-name required">Amount</span>
    <span class="param-type">decimal</span>
    <span class="param-desc">Kwota płatności.</span>
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
    <span class="param-name required">PaymentDate</span>
    <span class="param-type">datetime</span>
    <span class="param-desc">Data płatności.</span>
  </li>
  <li>
    <span class="param-name">PaymentTypeId</span>
    <span class="param-type">int</span>
    <span class="param-desc">Typ płatności (słownik).</span>
  </li>
  <li>
    <span class="param-name">SourceAccountNumber</span>
    <span class="param-type">string</span>
    <span class="param-desc">Numer rachunku źródłowego.</span>
  </li>
  <li>
    <span class="param-name">DestinationAccountNumber</span>
    <span class="param-type">string</span>
    <span class="param-desc">Numer rachunku docelowego.</span>
  </li>
  <li>
    <span class="param-name">Payer</span>
    <span class="param-type">string</span>
    <span class="param-desc">Nazwa płatnika.</span>
  </li>
  <li>
    <span class="param-name">Title</span>
    <span class="param-type">string</span>
    <span class="param-desc">Tytuł płatności.</span>
  </li>
  <li>
    <span class="param-name">Address</span>
    <span class="param-type">string</span>
    <span class="param-desc">Adres płatnika.</span>
  </li>
  <li>
    <span class="param-name">ExternalId</span>
    <span class="param-type">string</span>
    <span class="param-desc">Zewnętrzny identyfikator płatności.</span>
  </li>
  <li>
    <span class="param-name">OnlyInformational</span>
    <span class="param-type">bool</span>
    <span class="param-desc">Czy płatność jest wyłącznie informacyjna (nie wpływa na saldo).</span>
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
      "queueName": "Payment",
      "message": "{...}" // (1)
    }
    ```

    1. Pole `message` to **string zawierający JSON** — poniżej rozpakowana zawartość.

    ```json title="Zawartość pola message"
    {
      "Objects": [
        {
          "Amount": 66.66,
          "CurrencyId": 1,
          "ExchangeRate": 1,
          "PaymentDate": "2025-02-15T00:00:00",
          "PaymentTypeId": 15,
          "SourceAccountNumber": "12345678901234567890123456",
          "DestinationAccountNumber": "65432109876543210987654321",
          "Payer": "Jan Kowalski",
          "Title": "Opłata za usługi",
          "Address": "Warszawska 1 Warszawa",
          "ExternalId": "ID_123",
          "OnlyInformational": false,
          "SnapshotDate": {
            "Date": "2025-02-15T00:00:00"
          },
          "MatchKey": "Case_MK_1",
          "MatchKeyType": 6,
          "ObjectId": "00000000-0000-0000-0000-000000000000"
        }
      ],
      "ObjectsUpdateBehaviour": 6
    }
    ```

=== "Minimalny przykład"

    Dodanie prostej płatności na sprawie.

    ```json title="Koperta API"
    {
      "importId": "00000000-0000-0000-0000-000000000000",
      "queueName": "Payment",
      "message": "{...}"
    }
    ```

    ```json title="Zawartość pola message"
    {
      "Objects": [
        {
          "Amount": 100.00,
          "PaymentDate": "2025-02-15T00:00:00",
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
- Powiązane komunikaty: [Case](case.md) (sprawa), [Contract](contract.md) (wierzytelność), [Adjustment](adjustment.md) (korekty)

</div>
