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
    <span class="param-desc">Klucz wyszukiwania obiektu w DM. Patrz <a href="../#budowa-komunikatów">Budowa komunikatów</a>.</span>
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
    <span class="param-name required">Number</span>
    <span class="param-type">string</span>
    <span class="param-desc">Numer wierzytelności.</span>
  </li>
  <li>
    <span class="param-name required">Title</span>
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
    <span class="param-desc">ID waluty (słownik <code>waluta</code>). Puste = PLN.</span>
  </li>
  <li>
    <span class="param-name">BudgetGroupId</span>
    <span class="param-type">int</span>
    <span class="param-desc">ID grupy budżetowej (słownik <code>grupa_budzetowa</code>).</span>
  </li>
  <li>
    <span class="param-name">DPD</span>
    <span class="param-type">int</span>
    <span class="param-desc">Days Past Due (liczba dni przeterminowania).</span>
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
    <span class="param-type">long</span>
    <span class="param-desc">ID wersji obiektu w DM. <code>null</code> przy dodawaniu.</span>
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
    <span class="param-name">DueDate</span>
    <span class="param-type">datetime</span>
    <span class="param-desc">Data wymagalności dokumentu.</span>
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

`OrginalCreditorId` i `PortfolioId` muszą istnieć w słownikach (`kontrahent`, `umowa_kontrahent`) i zgadzać się z kontrahentem/portfelem importu. Wartości poniżej (`1`/`1`) odpowiadają środowisku testowemu.

=== "Minimalny"

    ```json title="Zawartość pola message"
    {
      "Objects": [
        {
          "ObjectId": "0481782f-712e-4a12-8eab-9fa5dace95ee",
          "MatchKey": "880000001",
          "MatchKeyType": 3,
          "Number": "UM/2024/001",
          "Title": "Umowa pożyczki 2024/001",
          "OrginalCreditorId": 1,
          "PortfolioId": 1
        }
      ],
      "ObjectsUpdateBehaviour": 6,
      "ObjectsAddedUserId": 5
    }
    ```

=== "Z dokumentem i księgowaniem"

    Wierzytelność z dokumentem (`DocumentsList`) i inicjalnym księgowaniem (`BookingsList`). `DocumentTypeId` ze słownika `dokument_typ`, `AccountTypeId` ze słownika `ksiegowanie_konto`, `SubAccountTypeId` ze słownika `ksiegowanie_konto_subkonto`.

    ```json title="Zawartość pola message"
    {
      "Objects": [
        {
          "ObjectId": "37620574-7ac6-4305-b4f3-8fc94d2f2da1",
          "MatchKey": "880000002",
          "MatchKeyType": 3,
          "Number": "UM/2024/002",
          "Title": "Umowa kredytowa 2024/002",
          "OrginalCreditorId": 1,
          "PortfolioId": 1,
          "CurrencyId": 1,
          "RegisterDate": "2023-01-15T00:00:00",
          "DocumentsList": {
            "Objects": [
              {
                "DocumentTypeId": 20,
                "Number": "FV/2023/100",
                "Title": "Kapitał",
                "CurrencyId": 1,
                "OpeningBalance": 5000.0,
                "IssueDate": "2023-02-01T00:00:00",
                "DueDate": "2023-03-01T00:00:00",
                "BookingsList": {
                  "Objects": [
                    { "AccountTypeId": 2, "Amount": 5000.0, "SubAccountTypeId": 0 }
                  ],
                  "ObjectsUpdateBehaviour": 6
                }
              }
            ],
            "ObjectsUpdateBehaviour": 6
          }
        }
      ],
      "ObjectsUpdateBehaviour": 6,
      "ObjectsAddedUserId": 5
    }
    ```

</div>

---

<div class="api-section" markdown>
<div class="api-section-title">Powiązania</div>

- Wysyłanie komunikatu — endpoint [EnqueueImportMessage](../funkcje-api/importy/enqueue-import-message.md)
- Macierz flag aktualizacyjnych — [ObjectsUpdateBehaviour](index.md#flagi-aktualizacyjne-obiektow-objectsupdatebehaviour)
- Powiązane komunikaty: [Customer](customer.md) (dłużnik), [Case](case.md) (sprawa), [ContractBalance](contract-balance.md) (zmiana salda), [Balance](balance.md) (nadpisanie salda)

</div>
