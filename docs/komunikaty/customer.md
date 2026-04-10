---
title: "Customer"
---

# Customer

<div class="msg-header">
  <span class="msg-type">Customer</span>
  <span class="msg-badge queue">kolejka: Customer</span>
  <span class="msg-badge db">dm_messages.debtor_details</span>
  <p class="msg-desc">Komunikat dodaje dłużnika z atrybutami, danymi kontaktowymi (adresy, telefony, e-mail), dokumentami tożsamości i właściwościami.</p>
</div>

---

<div class="api-section" markdown>
<div class="api-section-title">Struktura obiektu</div>

<div class="obj-tree">
  <span class="tree-root">Customer</span> <span class="tree-type">obiekt główny</span><br>
  <span class="tree-connector">├── </span><span class="tree-leaf">AttributesList</span> <span class="tree-type">lista atrybutów dłużnika</span><br>
  <span class="tree-connector">├── </span><span class="tree-leaf">EmailList</span> <span class="tree-type">adresy e-mail</span><br>
  <span class="tree-connector">├── </span><span class="tree-leaf">PhoneNumberList</span> <span class="tree-type">numery telefonów</span><br>
  <span class="tree-connector">├── </span><span class="tree-leaf">AddressList</span> <span class="tree-type">adresy korespondencyjne / zameldowania</span><br>
  <span class="tree-connector">├── </span><span class="tree-leaf">IdentityDocumentList</span> <span class="tree-type">dokumenty tożsamości</span><br>
  <span class="tree-connector">└── </span><span class="tree-leaf">CustomerPropertyList</span> <span class="tree-type">właściwości dłużnika</span>
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
<div class="api-section-title">Pola obiektu Customer</div>

<details class="collapsible-fields">
<summary>Customer — pola główne</summary>
<ul class="param-list">
  <li>
    <span class="param-name required">ObjectId</span>
    <span class="param-type">string</span>
    <span class="param-desc">Identyfikator obiektu do śledzenia statusu przetwarzania (GUID lub ID z systemu zewnętrznego).</span>
  </li>
  <li>
    <span class="param-name">DebtorPortfolioId</span>
    <span class="param-type">int</span>
    <span class="param-desc">ID portfela dłużnika.</span>
  </li>
  <li>
    <span class="param-name">ExternalId</span>
    <span class="param-type">string</span>
    <span class="param-desc">Zewnętrzny identyfikator dłużnika w systemie klienta.</span>
  </li>
  <li>
    <span class="param-name">CustomerTypeId</span>
    <span class="param-type">int</span>
    <span class="param-desc">Typ dłużnika (np. <code>1</code> — osoba fizyczna, <code>2</code> — firma).</span>
  </li>
  <li>
    <span class="param-name">FirstName</span>
    <span class="param-type">string</span>
    <span class="param-desc">Imię.</span>
  </li>
  <li>
    <span class="param-name">LastName</span>
    <span class="param-type">string</span>
    <span class="param-desc">Nazwisko.</span>
  </li>
  <li>
    <span class="param-name">SecondName</span>
    <span class="param-type">string</span>
    <span class="param-desc">Drugie imię.</span>
  </li>
  <li>
    <span class="param-name">CompanyFullName</span>
    <span class="param-type">string</span>
    <span class="param-desc">Pełna nazwa firmy (dla osób prawnych).</span>
  </li>
  <li>
    <span class="param-name">CompanyShortName</span>
    <span class="param-type">string</span>
    <span class="param-desc">Skrócona nazwa firmy.</span>
  </li>
  <li>
    <span class="param-name">SexId</span>
    <span class="param-type">int</span>
    <span class="param-desc">Płeć (<code>1</code> — mężczyzna, <code>2</code> — kobieta).</span>
  </li>
  <li>
    <span class="param-name">PESEL</span>
    <span class="param-type">string</span>
    <span class="param-desc">Numer PESEL.</span>
  </li>
  <li>
    <span class="param-name">REGON</span>
    <span class="param-type">string</span>
    <span class="param-desc">Numer REGON.</span>
  </li>
  <li>
    <span class="param-name">NIP</span>
    <span class="param-type">string</span>
    <span class="param-desc">Numer NIP.</span>
  </li>
  <li>
    <span class="param-name">KRS</span>
    <span class="param-type">string</span>
    <span class="param-desc">Numer KRS.</span>
  </li>
  <li>
    <span class="param-name">FamilyName</span>
    <span class="param-type">string</span>
    <span class="param-desc">Nazwisko rodowe.</span>
  </li>
  <li>
    <span class="param-name">Description</span>
    <span class="param-type">string</span>
    <span class="param-desc">Opis / komentarz do dłużnika.</span>
  </li>
  <li>
    <span class="param-name">BirthDate</span>
    <span class="param-type">datetime</span>
    <span class="param-desc">Data urodzenia.</span>
  </li>
  <li>
    <span class="param-name">DeathDate</span>
    <span class="param-type">datetime</span>
    <span class="param-desc">Data zgonu.</span>
  </li>
  <li>
    <span class="param-name">Death</span>
    <span class="param-type">bool</span>
    <span class="param-desc">Flaga zgonu.</span>
  </li>
  <li>
    <span class="param-name">MotherName</span>
    <span class="param-type">string</span>
    <span class="param-desc">Imię matki.</span>
  </li>
  <li>
    <span class="param-name">FatherName</span>
    <span class="param-type">string</span>
    <span class="param-desc">Imię ojca.</span>
  </li>
  <li>
    <span class="param-name">BirthPlace</span>
    <span class="param-type">string</span>
    <span class="param-desc">Miejsce urodzenia.</span>
  </li>
  <li>
    <span class="param-name">CompanyStatusId</span>
    <span class="param-type">int</span>
    <span class="param-desc">Status firmy (dla osób prawnych).</span>
  </li>
  <li>
    <span class="param-name">LiquidationDate</span>
    <span class="param-type">datetime</span>
    <span class="param-desc">Data likwidacji firmy.</span>
  </li>
  <li>
    <span class="param-name">BankruptcyDate</span>
    <span class="param-type">datetime</span>
    <span class="param-desc">Data ogłoszenia upadłości.</span>
  </li>
  <li>
    <span class="param-name">Bankruptcy</span>
    <span class="param-type">bool</span>
    <span class="param-desc">Flaga upadłości.</span>
  </li>
  <li>
    <span class="param-name">CountryId</span>
    <span class="param-type">int</span>
    <span class="param-desc">ID kraju (słownik).</span>
  </li>
  <li>
    <span class="param-name">SourceId</span>
    <span class="param-type">int</span>
    <span class="param-desc">ID źródła danych (słownik).</span>
  </li>
</ul>
</details>

<details class="collapsible-fields">
<summary>AttributesList — atrybuty dłużnika</summary>
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
  <li>
    <span class="param-name">ExternalId</span>
    <span class="param-type">string</span>
    <span class="param-desc">Zewnętrzny identyfikator atrybutu.</span>
  </li>
</ul>
</details>

<details class="collapsible-fields">
<summary>EmailList — adresy e-mail</summary>
<ul class="param-list">
  <li>
    <span class="param-name required">Email</span>
    <span class="param-type">string</span>
    <span class="param-desc">Adres e-mail.</span>
  </li>
  <li>
    <span class="param-name">SourceId</span>
    <span class="param-type">int</span>
    <span class="param-desc">ID źródła danych.</span>
  </li>
  <li>
    <span class="param-name">EmailTypeId</span>
    <span class="param-type">int</span>
    <span class="param-desc">Typ adresu e-mail (słownik).</span>
  </li>
  <li>
    <span class="param-name">ExternalId</span>
    <span class="param-type">string</span>
    <span class="param-desc">Zewnętrzny identyfikator.</span>
  </li>
  <li>
    <span class="param-name">AdditionalInformation</span>
    <span class="param-type">string</span>
    <span class="param-desc">Dodatkowe informacje.</span>
  </li>
</ul>
</details>

<details class="collapsible-fields">
<summary>PhoneNumberList — numery telefonów</summary>
<ul class="param-list">
  <li>
    <span class="param-name required">PhoneNumber</span>
    <span class="param-type">string</span>
    <span class="param-desc">Numer telefonu.</span>
  </li>
  <li>
    <span class="param-name">SourceId</span>
    <span class="param-type">int</span>
    <span class="param-desc">ID źródła danych.</span>
  </li>
  <li>
    <span class="param-name">Comment</span>
    <span class="param-type">string</span>
    <span class="param-desc">Komentarz.</span>
  </li>
  <li>
    <span class="param-name">PhoneNumberTypeId</span>
    <span class="param-type">int</span>
    <span class="param-desc">Typ numeru telefonu (słownik).</span>
  </li>
  <li>
    <span class="param-name">ExternalId</span>
    <span class="param-type">string</span>
    <span class="param-desc">Zewnętrzny identyfikator.</span>
  </li>
</ul>
</details>

<details class="collapsible-fields">
<summary>AddressList — adresy</summary>
<ul class="param-list">
  <li>
    <span class="param-name required">AddressTypeId</span>
    <span class="param-type">int</span>
    <span class="param-desc">Typ adresu (słownik).</span>
  </li>
  <li>
    <span class="param-name">SourceId</span>
    <span class="param-type">int</span>
    <span class="param-desc">ID źródła danych.</span>
  </li>
  <li>
    <span class="param-name">Country</span>
    <span class="param-type">string</span>
    <span class="param-desc">Kraj.</span>
  </li>
  <li>
    <span class="param-name">Street</span>
    <span class="param-type">string</span>
    <span class="param-desc">Ulica.</span>
  </li>
  <li>
    <span class="param-name">HouseNumber</span>
    <span class="param-type">string</span>
    <span class="param-desc">Numer domu.</span>
  </li>
  <li>
    <span class="param-name">ApartmentNumber</span>
    <span class="param-type">string</span>
    <span class="param-desc">Numer mieszkania.</span>
  </li>
  <li>
    <span class="param-name">PostalCode</span>
    <span class="param-type">string</span>
    <span class="param-desc">Kod pocztowy.</span>
  </li>
  <li>
    <span class="param-name">City</span>
    <span class="param-type">string</span>
    <span class="param-desc">Miasto.</span>
  </li>
  <li>
    <span class="param-name">PostalOffice</span>
    <span class="param-type">string</span>
    <span class="param-desc">Poczta.</span>
  </li>
  <li>
    <span class="param-name">AdditionalInformation</span>
    <span class="param-type">string</span>
    <span class="param-desc">Dodatkowe informacje.</span>
  </li>
  <li>
    <span class="param-name">RegionId</span>
    <span class="param-type">int</span>
    <span class="param-desc">ID regionu (słownik).</span>
  </li>
  <li>
    <span class="param-name">ExternalId</span>
    <span class="param-type">string</span>
    <span class="param-desc">Zewnętrzny identyfikator.</span>
  </li>
</ul>
</details>

<details class="collapsible-fields">
<summary>IdentityDocumentList — dokumenty tożsamości</summary>
<ul class="param-list">
  <li>
    <span class="param-name required">DocumentTypeId</span>
    <span class="param-type">int</span>
    <span class="param-desc">Typ dokumentu tożsamości (słownik).</span>
  </li>
  <li>
    <span class="param-name">Series</span>
    <span class="param-type">string</span>
    <span class="param-desc">Seria dokumentu.</span>
  </li>
  <li>
    <span class="param-name">Number</span>
    <span class="param-type">string</span>
    <span class="param-desc">Numer dokumentu.</span>
  </li>
  <li>
    <span class="param-name">IssueDate</span>
    <span class="param-type">datetime</span>
    <span class="param-desc">Data wydania.</span>
  </li>
  <li>
    <span class="param-name">ExpiryDate</span>
    <span class="param-type">datetime</span>
    <span class="param-desc">Data ważności.</span>
  </li>
  <li>
    <span class="param-name">IssuingAuthority</span>
    <span class="param-type">string</span>
    <span class="param-desc">Organ wydający.</span>
  </li>
  <li>
    <span class="param-name">SourceId</span>
    <span class="param-type">int</span>
    <span class="param-desc">ID źródła danych.</span>
  </li>
</ul>
</details>

<details class="collapsible-fields">
<summary>CustomerPropertyList — właściwości dłużnika</summary>
<ul class="param-list">
  <li>
    <span class="param-name required">TypeId</span>
    <span class="param-type">int</span>
    <span class="param-desc">ID typu właściwości (słownik).</span>
  </li>
  <li>
    <span class="param-name required">SubtypeId</span>
    <span class="param-type">int</span>
    <span class="param-desc">ID podtypu właściwości (słownik).</span>
  </li>
  <li>
    <span class="param-name">ValidFrom</span>
    <span class="param-type">datetime</span>
    <span class="param-desc">Data początku obowiązywania.</span>
  </li>
  <li>
    <span class="param-name">ValidTo</span>
    <span class="param-type">datetime</span>
    <span class="param-desc">Data końca obowiązywania.</span>
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
      "queueName": "Customer",
      "message": "{...}" // (1)
    }
    ```

    1. Pole `message` to **string zawierający JSON** — poniżej rozpakowana zawartość.

    ```json title="Zawartość pola message"
    {
      "Objects": [
        {
          "DebtorPortfolioId": 1,
          "ExternalId": "EXT12345",
          "CustomerTypeId": 1,
          "FirstName": "John",
          "LastName": "Doe",
          "CompanyFullName": "John Doe Enterprises Ltd.",
          "CompanyShortName": "JDE Ltd.",
          "SexId": 1,
          "PESEL": "12345678901",
          "REGON": "987654321",
          "NIP": "123-45-67-890",
          "KRS": "0000123456",
          "FamilyName": "Doe",
          "Description": "A valued customer",
          "BirthDate": "1985-05-20T00:00:00",
          "DeathDate": null,
          "SecondName": "Michael",
          "MotherName": "Jane Doe",
          "FatherName": "Richard Doe",
          "BirthPlace": "New York",
          "CompanyStatusId": null,
          "LiquidationDate": null,
          "BankruptcyDate": null,
          "CountryId": 1,
          "Death": false,
          "Bankruptcy": false,
          "SourceId": 5,
          "AttributesList": {
            "Objects": [
              { "TypeId": 2, "Value": "test", "ExternalId": null }
            ],
            "ObjectsUpdateBehaviour": 6
          },
          "EmailList": {
            "Objects": [
              {
                "Email": "jan.kowalski@example.com",
                "SourceId": 1,
                "EmailTypeId": 1,
                "ExternalId": "1",
                "AdditionalInformation": ""
              }
            ],
            "ObjectsUpdateBehaviour": 6
          },
          "PhoneNumberList": {
            "Objects": [
              {
                "PhoneNumber": "600123456",
                "SourceId": 1,
                "Comment": "",
                "PhoneNumberTypeId": 1,
                "ExternalId": "1"
              }
            ],
            "ObjectsUpdateBehaviour": 6
          },
          "AddressList": {
            "Objects": [
              {
                "AddressTypeId": 12,
                "SourceId": 1,
                "Country": "Polska",
                "Street": "Marszałkowska",
                "HouseNumber": "1",
                "ApartmentNumber": "10",
                "PostalCode": "00-001",
                "City": "Warszawa",
                "PostalOffice": "Warszawa",
                "AdditionalInformation": "",
                "RegionId": 1,
                "ExternalId": "1"
              }
            ],
            "ObjectsUpdateBehaviour": 6
          },
          "IdentityDocumentList": {
            "Objects": [
              {
                "DocumentTypeId": 1,
                "Series": "ABC",
                "Number": "123456",
                "IssueDate": "2020-01-15T00:00:00",
                "ExpiryDate": "2030-01-15T00:00:00",
                "IssuingAuthority": "Prezydent m.st. Warszawy",
                "SourceId": 1
              }
            ],
            "ObjectsUpdateBehaviour": 6
          },
          "CustomerPropertyList": {
            "Objects": [
              {
                "TypeId": 3,
                "SubtypeId": 11,
                "ValidFrom": "1985-05-20T00:00:00",
                "ValidTo": null
              }
            ],
            "ObjectsUpdateBehaviour": 6
          },
          "MatchKeyType": 2,
          "ObjectId": "00000000-0000-0000-0000-000000000000",
          "MatchKey": "Customer_MK_1"
        }
      ],
      "ObjectsUpdateBehaviour": 6
    }
    ```

=== "Minimalny przykład"

    Tylko wymagane pola — dodanie osoby fizycznej bez list zagnieżdżonych.

    ```json title="Koperta API"
    {
      "importId": "00000000-0000-0000-0000-000000000000",
      "queueName": "Customer",
      "message": "{...}"
    }
    ```

    ```json title="Zawartość pola message"
    {
      "Objects": [
        {
          "CustomerTypeId": 1,
          "FirstName": "Jan",
          "LastName": "Kowalski",
          "PESEL": "83041100112",
          "MatchKeyType": 2,
          "ObjectId": "00000000-0000-0000-0000-000000000000",
          "MatchKey": "774"
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
- Powiązane komunikaty: [Case](case.md) (sprawa dłużnika), [Contract](contract.md) (wierzytelność), [Signature](signature.md) (podpis)

</div>
