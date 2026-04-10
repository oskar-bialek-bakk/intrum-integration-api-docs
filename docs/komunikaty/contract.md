---
title: "Contract"
---

# Contract

<div class="msg-header">
  <span class="msg-type">Contract</span>
  <span class="msg-badge queue">kolejka: Contract</span>
  <span class="msg-badge db">dm_messages.contract_details</span>
  <p class="msg-desc">Komunikat dodaje wierzytelność z inicjalnym stanem finansowym.</p>
</div>

---

<div class="api-section" markdown>
<div class="api-section-title">Struktura obiektu</div>

<div class="obj-tree">
  <span class="tree-root">Contract</span> <span class="tree-type">obiekt główny</span><br>
  <span class="tree-connector">├── </span><span class="tree-leaf">AttributesList</span> <span class="tree-type">lista atrybutów wierzytelności</span><br>
  <span class="tree-connector">└── </span><span class="tree-leaf">DocumentsList</span> <span class="tree-type">dokumenty księgowe</span><br>
  <span class="tree-connector">&nbsp;&nbsp;&nbsp;&nbsp;├── </span><span class="tree-leaf">AttributesList</span> <span class="tree-type">atrybuty dokumentu</span><br>
  <span class="tree-connector">&nbsp;&nbsp;&nbsp;&nbsp;└── </span><span class="tree-leaf">BookingsList</span> <span class="tree-type">księgowania na dokumencie</span>
</div>

Każda lista zagnieżdżona ma własne pole `ObjectsUpdateBehaviour` — patrz [macierz flag aktualizacyjnych](index.md#flagi-aktualizacyjne-obiektow-objectsupdatebehaviour).

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
    </ul>
  </li>
</ul>

</div>

---

<div class="api-section">
<div class="api-section-title">Pola obiektu Contract</div>

<details class="collapsible-fields">
<summary>Contract — pola główne</summary>
<ul class="param-list">
  <li>
    <span class="param-name required">ObjectId</span>
    <span class="param-type">string</span>
    <span class="param-desc">Identyfikator obiektu do śledzenia statusu przetwarzania.</span>
  </li>
  <li>
    <span class="param-name">Number</span>
    <span class="param-type">string</span>
    <span class="param-desc">Numer wierzytelności.</span>
  </li>
  <li>
    <span class="param-name">Title</span>
    <span class="param-type">string</span>
    <span class="param-desc">Tytuł wierzytelności.</span>
  </li>
  <li>
    <span class="param-name">TypeId</span>
    <span class="param-type">int</span>
    <span class="param-desc">Typ wierzytelności (słownik).</span>
  </li>
  <li>
    <span class="param-name">ProductTypeId</span>
    <span class="param-type">int</span>
    <span class="param-desc">Typ produktu (słownik).</span>
  </li>
  <li>
    <span class="param-name">PortfolioId</span>
    <span class="param-type">int</span>
    <span class="param-desc">ID portfela.</span>
  </li>
  <li>
    <span class="param-name">OrginalCreditorId</span>
    <span class="param-type">int</span>
    <span class="param-desc">ID pierwotnego wierzyciela (słownik).</span>
  </li>
  <li>
    <span class="param-name">CurrencyId</span>
    <span class="param-type">int</span>
    <span class="param-desc">ID waluty (słownik).</span>
  </li>
  <li>
    <span class="param-name">RegisterDate</span>
    <span class="param-type">datetime</span>
    <span class="param-desc">Data rejestracji wierzytelności.</span>
  </li>
  <li>
    <span class="param-name">EndDate</span>
    <span class="param-type">datetime</span>
    <span class="param-desc">Data zakończenia.</span>
  </li>
  <li>
    <span class="param-name">VersionId</span>
    <span class="param-type">int</span>
    <span class="param-desc">ID wersji.</span>
  </li>
  <li>
    <span class="param-name">SnapshotDate</span>
    <span class="param-type">object</span>
    <span class="param-desc">Data migawki — obiekt z polem <code>Date</code> (datetime).</span>
  </li>
</ul>
</details>

<details class="collapsible-fields">
<summary>AttributesList — atrybuty wierzytelności</summary>
<ul class="param-list">
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
</ul>
</details>

<details class="collapsible-fields">
<summary>DocumentsList — dokumenty księgowe</summary>
<ul class="param-list">
  <li>
    <span class="param-name required">DocumentTypeId</span>
    <span class="param-type">int</span>
    <span class="param-desc">Typ dokumentu księgowego (słownik).</span>
  </li>
  <li>
    <span class="param-name">MatchKey</span>
    <span class="param-type">string</span>
    <span class="param-desc">Zewnętrzny identyfikator dokumentu.</span>
  </li>
  <li>
    <span class="param-name">Number</span>
    <span class="param-type">string</span>
    <span class="param-desc">Numer dokumentu.</span>
  </li>
  <li>
    <span class="param-name">Title</span>
    <span class="param-type">string</span>
    <span class="param-desc">Tytuł dokumentu.</span>
  </li>
  <li>
    <span class="param-name">Description</span>
    <span class="param-type">string</span>
    <span class="param-desc">Opis dokumentu.</span>
  </li>
  <li>
    <span class="param-name">Comment</span>
    <span class="param-type">string</span>
    <span class="param-desc">Komentarz.</span>
  </li>
  <li>
    <span class="param-name">CurrencyId</span>
    <span class="param-type">int</span>
    <span class="param-desc">ID waluty dokumentu.</span>
  </li>
  <li>
    <span class="param-name">ExchangeRate</span>
    <span class="param-type">decimal</span>
    <span class="param-desc">Kurs wymiany waluty.</span>
  </li>
  <li>
    <span class="param-name">OpeningBalance</span>
    <span class="param-type">decimal</span>
    <span class="param-desc">Saldo otwarcia dokumentu.</span>
  </li>
  <li>
    <span class="param-name">IssueDate</span>
    <span class="param-type">datetime</span>
    <span class="param-desc">Data wystawienia.</span>
  </li>
  <li>
    <span class="param-name">LimitationDate</span>
    <span class="param-type">datetime</span>
    <span class="param-desc">Data przedawnienia.</span>
  </li>
  <li>
    <span class="param-name">InterestTypeId</span>
    <span class="param-type">int</span>
    <span class="param-desc">Typ odsetek (słownik).</span>
  </li>
  <li>
    <span class="param-name">InterestAccountId</span>
    <span class="param-type">int</span>
    <span class="param-desc">ID konta odsetkowego.</span>
  </li>
  <li>
    <span class="param-name">InterestCalculationDate</span>
    <span class="param-type">datetime</span>
    <span class="param-desc">Data naliczenia odsetek.</span>
  </li>
</ul>
</details>

<details class="collapsible-fields">
<summary>BookingsList — księgowania na dokumencie</summary>
<ul class="param-list">
  <li>
    <span class="param-name required">AccountTypeId</span>
    <span class="param-type">int</span>
    <span class="param-desc">Typ konta księgowego (słownik).</span>
  </li>
  <li>
    <span class="param-name required">Amount</span>
    <span class="param-type">decimal</span>
    <span class="param-desc">Kwota księgowania.</span>
  </li>
  <li>
    <span class="param-name">DueDate</span>
    <span class="param-type">datetime</span>
    <span class="param-desc">Data wymagalności.</span>
  </li>
  <li>
    <span class="param-name">SubAccountTypeId</span>
    <span class="param-type">int</span>
    <span class="param-desc">Typ subkonta (słownik).</span>
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
      "queueName": "Contract",
      "message": "{...}" // (1)
    }
    ```

    1. Pole `message` to **string zawierający JSON** — poniżej rozpakowana zawartość.

    ```json title="Zawartość pola message"
    {
      "Objects": [
        {
          "Number": "521578",
          "Title": "Wydanie gazomierza",
          "TypeId": 1,
          "ProductTypeId": 1,
          "PortfolioId": 1,
          "OrginalCreditorId": 1,
          "CurrencyId": 1,
          "RegisterDate": "1900-01-01T00:00:00",
          "EndDate": null,
          "VersionId": null,
          "SnapshotDate": {
            "Date": "2025-02-15T00:00:00"
          },
          "AttributesList": {
            "Objects": [
              { "TypeId": 2, "Value": "10" }
            ],
            "ObjectsUpdateBehaviour": 6
          },
          "DocumentsList": {
            "Objects": [
              {
                "DocumentTypeId": 30,
                "MatchKey": "123456",
                "Number": "457457",
                "Title": "Wydanie gazomierza",
                "Description": "Windykacja twarda - wydanie gazomierzy",
                "Comment": "Dodatkowy komentarz",
                "CurrencyId": 1,
                "ExchangeRate": 1,
                "OpeningBalance": 250,
                "IssueDate": "2023-03-16T00:00:00",
                "LimitationDate": "2023-03-16T00:00:00",
                "InterestTypeId": 1,
                "InterestAccountId": 1,
                "InterestCalculationDate": "2023-04-06T00:00:00",
                "AttributesList": {
                  "Objects": [
                    { "TypeId": 2, "Value": "1" }
                  ],
                  "ObjectsUpdateBehaviour": 6
                },
                "BookingsList": {
                  "Objects": [
                    {
                      "AccountTypeId": 2,
                      "Amount": 100,
                      "DueDate": "2023-04-06T00:00:00",
                      "SubAccountTypeId": 0
                    }
                  ],
                  "ObjectsUpdateBehaviour": 6
                }
              }
            ],
            "ObjectsUpdateBehaviour": 6
          },
          "MatchKey": "Contract_MK_1",
          "MatchKeyType": 3,
          "ObjectId": "00000000-0000-0000-0000-000000000000"
        }
      ],
      "ObjectsUpdateBehaviour": 6
    }
    ```

=== "Minimalny przykład"

    Tylko wymagane pola — dodanie wierzytelności bez dokumentów.

    ```json title="Koperta API"
    {
      "importId": "00000000-0000-0000-0000-000000000000",
      "queueName": "Contract",
      "message": "{...}"
    }
    ```

    ```json title="Zawartość pola message"
    {
      "Objects": [
        {
          "Number": "UM/2024/001",
          "TypeId": 1,
          "CurrencyId": 1,
          "MatchKey": "Contract_MK_1",
          "MatchKeyType": 3,
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
- Powiązane komunikaty: [Customer](customer.md) (dłużnik), [Case](case.md) (sprawa), [ContractBalance](contract-balance.md) (zmiana salda), [Balance](balance.md) (nadpisanie salda)

</div>
